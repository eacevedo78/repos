FROM ruby:2.7.1-buster

#Instalar jtds para sqlserver
ADD freetds-1.2.3.tar.gz /opt/
RUN cd /opt/freetds-1.2.3 && ./configure && make  && make install
RUN rm -rf /opt/freetds-1.2.3/ && echo 'root:admin123' | chpasswd

ENV DEBIAN_FRONTEND=noninteractive
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn

#Instalar nodejs, yarn y gemas para sqlserver, sqlite3, mysql2 y postgresql
RUN curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt install -y yarn && \
    gem install tiny_tds mysql2 sqlite3 pg

RUN ls /root
# Parche tomado de https://github.com/eclipse/che-devfile-registry/blob/master/arbitrary-users-patch/Dockerfile
# para que funcione con eclipse che worspaces
USER 0
# Set permissions on /etc/passwd and /home to allow arbitrary users to write
COPY --chown=0:0 entrypoint.sh /
RUN mkdir -p /home/user && chgrp -R 0 /home && chmod -R g=u /etc/passwd /etc/group /home && chmod +x /entrypoint.sh

USER 10001
ENV HOME=/home/user
WORKDIR /projects
ENTRYPOINT [ "/entrypoint.sh" ]
CMD ["tail", "-f", "/dev/null"]

