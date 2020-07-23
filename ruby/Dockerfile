FROM alpine:3.12.0

ADD freetds-1.2.3.tar.gz /opt/

RUN apk add alpine-sdk linux-headers bash nodejs yarn openssl-dev zlib-dev && \
    cd /opt/freetds-1.2.3 && ./configure && make  && make install && \
    rm -rf /opt/freetds-1.2.3/

SHELL ["/bin/bash","-c"]

#Codigo tomado de https://github.com/eclipse/che-devfile-registry/blob/master/arbitrary-users-patch/Dockerfile
# para que el contenedor trabaje con eclipse che

COPY --chown=0:0 entrypoint.sh /
RUN mkdir -p /home/user && chgrp -R 0 /home && chmod -R g=u /etc/passwd /etc/group /home && chmod +x /entrypoint.sh

USER 10001
ENV HOME=/home/user
WORKDIR /projects

#Instalar el rbenv
ENV PATH=$PATH:$HOME/.rbenv/bin:$HOME/.rbenv/shims
RUN curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer | bash && \
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc && \
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc && \
    #source ~/.bashrc
    rbenv install 2.7.1 && \
    rbenv global 2.7.1


ENTRYPOINT [ "/entrypoint.sh" ]

CMD tail -f /dev/null
