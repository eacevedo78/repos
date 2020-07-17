FROM ruby:2.7.1-buster

#Instalar jtds para sqlserver, las librerias para postgres, mysql y sqlite3 ya vienen en la imagen ruby:2.7.1-buster
ADD freetds-1.2.3.tar.gz /opt/
RUN cd /opt/freetds-1.2.3 && ./configure && make  && make install
RUN rm -rf /opt/freetds-1.2.3/ && echo 'root:admin123' | chpasswd

ENV DEBIAN_FRONTEND=noninteractive
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn

#Instalar nodejs, yarn
RUN curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt install -y yarn sudo && \
    apt -y autoremove && apt -y clean

COPY --chown=0:0 entrypoint.sh /

# Crear usuario 'user' y darle permisos
RUN echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    sudo useradd -u 1000 -G users,sudo,root -d /home/user --shell /bin/bash -m user && \
    sudo usermod -p "*" user && chmod +x /entrypoint.sh

# Usar por default el usuario 'user' y asignar home y directorio de trabajo
USER user
ENV HOME=/home/user
WORKDIR /projects

#Dar permisos al usuario user a ciertas carpetas
# Set permissions on /etc/passwd and /home to allow arbitrary users to write
RUN for f in "/home/user" "/etc/passwd" "/etc/group" "/projects"; do\
        sudo   chgrp -R 0 ${f} && \
        sudo   chmod -R g+rwX ${f}; \
    done

ENTRYPOINT [ "/entrypoint.sh" ]
CMD ["tail", "-f", "/dev/null"]

