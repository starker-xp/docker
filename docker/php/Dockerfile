FROM php:fpm

ENV TERM=xterm

RUN apt-get update
RUN apt-get install -y wget

RUN echo "deb http://packages.dotdeb.org jessie all" > /etc/apt/sources.list.d/dotdeb.list
RUN wget -O- https://www.dotdeb.org/dotdeb.gpg | apt-key add -

RUN apt-get update

RUN apt-get install -y mysql-client php7.0 php-mysql php7.0-mysql php7.0-curl php7.0-json php7.0-gd php7.0-mcrypt php7.0-msgpack php7.0-memcached php7.0-intl php7.0-mbstring php7.0-xml php7.0-zip
RUN docker-php-ext-install pdo pdo_mysql

RUN apt-get install -y git nano acl systemd systemd-sysv curl
RUN docker-php-ext-install pdo pdo_mysql
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony 
RUN chmod a+x /usr/local/bin/symfony

RUN curl -LsS https://phar.phpunit.de/phpunit.phar -o /usr/local/bin/phpunit
RUN chmod a+x /usr/local/bin/phpunit