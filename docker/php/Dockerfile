FROM php:fpm

ENV TERM=xterm

RUN apt-get update
RUN apt-get install -y wget

RUN echo "deb http://packages.dotdeb.org jessie all" > /etc/apt/sources.list.d/dotdeb.list
RUN wget -O- https://www.dotdeb.org/dotdeb.gpg | apt-key add -

RUN apt-get update

RUN buildDeps=" \
        build-essential \
        libmcrypt-dev \
        libgcrypt11-dev \
        libpng12-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libicu-dev \
        zlib1g-dev \
        libmemcached-dev \
    " \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        $buildDeps \
        vim \
        libicu52 \
        libmcrypt4 \
        libjpeg62-turbo \
        libfreetype6 \
        libmemcachedutil2 \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd 

	
RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
    mysqli \
    zip \
    mbstring \
    bcmath
RUN docker-php-ext-install \
    intl \
    opcache \
    pcntl \
    gd \
    iconv \
    mcrypt \
    exif
	
RUN apt-get install -y mysql-client php7.0 php-mysql php7.0-mysql php7.0-curl php7.0-json php7.0-gd php7.0-mcrypt php7.0-msgpack php7.0-memcached php7.0-intl php7.0-mbstring php7.0-xml php7.0-zip git nano acl systemd systemd-sysv curl

RUN apt-get install php7.0-xdebug -y
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN echo 'xdebug.remote_enable=1' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.remote_handler=dbgp' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.remote_mode=req' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.remote_port=9000' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.idekey=phpstorm-xdebug' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.remote_autostart=1' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.remote_connect_back=1' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.remote_host=127.0.0.1' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo 'xdebug.remote_log=/var/log/xdebug_remote.log' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony 
RUN chmod a+x /usr/local/bin/symfony

RUN curl -LsS https://phar.phpunit.de/phpunit.phar -o /usr/local/bin/phpunit
RUN chmod a+x /usr/local/bin/phpunit

RUN version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") \
    && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/linux/amd64/$version \
    && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp \
    && mv /tmp/blackfire-*.so $(php -r "echo ini_get('extension_dir');")/blackfire.so \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > $PHP_INI_DIR/conf.d/blackfire.ini
