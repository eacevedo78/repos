FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn
RUN apt-get update && apt-get -y upgrade && \
    apt-get -y install \
    sudo locales \
    unzip openssh-server \
    libsqlite3-dev libmysqlclient-dev libpq-dev \
    ca-certificates \
    curl build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev \
    git libncurses5-dev libffi-dev  \
    bash-completion && \
    mkdir /var/run/sshd && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
    echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    # Adding user to the 'root' is a workaround for https://issues.jboss.org/browse/CDK-305
    useradd -u 1000 -G users,sudo,root -d /home/user --shell /bin/bash -m user && \
    usermod -p "*" user && \
    apt-get clean && \
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*


#Instalar jtds para sqlserver, las librerias para postgres, mysql y sqlite3
ADD freetds-1.2.3.tar.gz /opt/
RUN cd /opt/freetds-1.2.3 && ./configure && make  && make install
RUN rm -rf /opt/freetds-1.2.3/ #&& echo 'root:admin123' | chpasswd

#ENV DEBIAN_FRONTEND=noninteractive
#ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn

#Instalar nodejs, yarn
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt install -y yarn && \
    apt -y autoremove && apt -y clean

ENV LANG en_US.UTF-8

RUN sudo locale-gen en_US.UTF-8 && \
    cd /home/user && ls -la


# The following instructions set the right
# permissions and scripts to allow the container
# to be run by an arbitrary user (i.e. a user
# that doesn't already exist in /etc/passwd)
ENV HOME /home/user
ENV PATH $PATH:/$HOME/.rbenv/bin:$HOME/.rbenv/shims
RUN for f in "/home/user" "/etc/passwd" "/etc/group" "/projects"; do\
           sudo chgrp -R 0 ${f} && \
           sudo chmod -R g+rwX ${f}; \
        done && \
    # Generate passwd.template \
    cat /etc/passwd | \
    sed s#user:x.*#user:x:\${USER_ID}:\${GROUP_ID}::\${HOME}:/bin/bash#g \
    > /home/user/passwd.template && \
    # Generate group.template \
    cat /etc/group | \
    sed s#root:x:0:#root:x:0:0,\${USER_ID}:#g \
    > /home/user/group.template && \
    sudo sed -ri 's/StrictModes yes/StrictModes no/g' /etc/ssh/sshd_config

COPY entrypoint.sh /home/user/entrypoint.sh
#Instalar rbenv
#Descargar rbenv de github
#SHELL ["/bin/bash", "-c"]
RUN curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer | sudo bash && \
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc && \
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc && \
    #source ~/.bashrc
    sudo rbenv install 2.7.1 && \
    sudo rbenv global 2.7.1 && sudo chmod +x /home/user/entrypoint.sh

USER user
EXPOSE 22 4403
WORKDIR /projects

ENTRYPOINT ["/home/user/entrypoint.sh"]
CMD tail -f /dev/null
