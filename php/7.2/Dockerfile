# Copyright (c) 2012-2016 Codenvy, S.A.
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# which accompanies this distribution, and is available at
# http://www.eclipse.org/legal/epl-v10.html
# Contributors:
# Codenvy, S.A. - initial API and implementation

FROM eclipse/stack-base:ubuntu
ENV DEBIAN_FRONTEND noninteractive

ADD instantclient-basic-linux.x64-12.2.0.1.0.zip /opt/
RUN cd /opt/ && sudo unzip instantclient-basic-linux.x64-12.2.0.1.0.zip && sudo rm instantclient-basic-linux.x64-12.2.0.1.0.zip
RUN sudo add-apt-repository ppa:ondrej/php

RUN sudo apt-get update && \
    sudo apt-get install -y php7.2 \
    apache2 \
    php \
    php-mcrypt \
    php-curl \
    php-mysql \
    php-pgsql \
    php-gd \
    libapache2-mod-php \
    php-cli \
    php-json \
    php-cgi \
    php-sqlite3 \
    php-mbstring \
    php-xml \
    php-dev \
    php-zip \
    php-pear

ENV ORACLE_HOME /opt/instantclient_12_2
ENV PHP_INI /

RUN sudo pecl install oci8

RUN sudo sed -i 's/\/var\/www\/html/\/projects/g'  /etc/apache2/sites-available/000-default.conf && \
    sudo sed -i 's/\/var\/www/\/projects/g'  /etc/apache2/apache2.conf && \
    sudo sed -i 's/None/All/g' /etc/apache2/sites-available/000-default.conf && \
    echo "ServerName localhost" | sudo tee -a /etc/apache2/apache2.conf && \
    sudo a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer && \
    sudo chown -R user:user ~/.composer && \
    composer global require bamarni/symfony-console-autocomplete && \
    ~/.composer/vendor/bamarni/symfony-console-autocomplete/symfony-autocomplete --shell bash composer | sudo tee /etc/bash_completion.d/composer && \
    sudo wget -qO /usr/local/bin/phpunit https://phar.phpunit.de/phpunit.phar && sudo chmod +x /usr/local/bin/phpunit \
    sudo apt-get clean && \
    sudo apt-get -y autoremove && \
    sudo apt-get -y clean && \
    sudo rm -rf /var/lib/apt/lists/* &&

RUN curl -sL https://deb.nodesource.com/setup_9.x | sudo bash - && \
    sudo apt-get update && \
    sudo apt-get install -y nodejs

RUN sudo composer global require "laravel/installer"

ENV PATH /home/user/.composer/vendor/bin:$PATH

EXPOSE 80 8000

CMD sudo chown -R www-data:www-data /projects && \
    sudo chmod -R 777 /projects && \
    tail -f /dev/null
