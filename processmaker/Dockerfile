FROM simplific/nginx-php

ENV PROCESSMAKER_VERSION 2.5.2
ADD "processmaker-${PROCESSMAKER_VERSION}.tar.gz" /opt/
ADD 01-processmaker.conf /etc/nginx/conf.d/01-processmaker.conf

RUN 	apk --no-cache --update add php-ctype php-cli php-curl php-soap php-ldap php-dom php-mysql freetds php-mssql \
	&& chown -R nginx:www-data /opt/processmaker \
	&& cd /opt/processmaker/workflow/engine \
	&& ln -s ../../gulliver/bin/gulliver gulliver \
	&& echo "*/5 * * * * php -f /opt/processmaker/workflow/engine/bin/cron.php +force" >> /var/spool/cron/crontabs/root

VOLUME "/opt/processmaker/"
WORKDIR "/opt/processmaker/workflow/engine"

