FROM ruby:3.0.1-alpine3.13

ADD freetds-1.2.21.tar.gz /opt/

#RUN apk add alpine-sdk linux-headers bash nodejs yarn openssl-dev zlib-dev tzdata \
#    mariadb-connector-c-dev sqlite-dev postgresql-dev && \
#    cd /opt/freetds-1.2.3 && ./configure && make  && make install && \
#    rm -rf /opt/freetds-1.2.3/

RUN apk add bash build-base tzdata mariadb-connector-c-dev sqlite-dev postgresql-dev nodejs yarn && \
    cd /opt/freetds-1.2.21 && ./configure && make  && make install && \
    rm -rf /opt/freetds-1.2.21/

SHELL ["/bin/bash","-c"]

#RUN addgroup -S user 
#RUN adduser -u 10001 -G user -h /home/user -S user

#Codigo tomado de https://github.com/eclipse/che-devfile-registry/blob/master/arbitrary-users-patch/Dockerfile
# para que el contenedor trabaje con eclipse che

#ENV HOME=/home/user

COPY --chown=0:0 entrypoint.sh /
RUN mkdir -p /home/user && chgrp -R 0 /home && chmod -R g=u /etc/passwd /etc/group /home && chmod +x /entrypoint.sh

RUN addgroup -S user 
RUN adduser -u 10001 -G user -h /home/user -S user

USER 10001
#USER user
ENV HOME=/home/user
ENV GEM_HOME=$HOME/.gem
ENV PATH=$PATH:$HOME/.rbenv/bin:$HOME/.rbenv/shims:$HOME/.gem/bin
WORKDIR /projects

ENTRYPOINT [ "/entrypoint.sh" ]

CMD tail -f /dev/null
