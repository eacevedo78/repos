FROM eboraas/apache:latest
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && apt-get -y install php php-mysql libapache2-mod-php cron php7.3-xml php7.3-curl && apt-get clean && rm -rf /var/lib/apt/lists/*
RUN /usr/sbin/a2enmod mpm_prefork

ADD https://github.com/phpservermon/phpservermon/releases/download/v3.4.5/phpservermon-3.4.5.zip /var/www/html

EXPOSE 80
EXPOSE 443

RUN /etc/init.d/cron start
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
