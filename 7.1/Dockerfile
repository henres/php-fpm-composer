# Duplicate of dockerfile for 7.1 from https://github.com/maxpou/docker-symfony
FROM php:7.1-fpm

MAINTAINER Henri d Auvigny <henri.dauvigny@gmail.com>

RUN apt-get update && apt-get install -y \
    git \
    unzip
    
COPY ./php.ini /usr/local/etc/php/php.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime
RUN "date"

# Install Software
RUN pecl channel-update pecl.php.net

RUN apt-get update && apt-get install -y \
		libicu-dev \
		wget \
	&& docker-php-ext-install \
		intl \
		mbstring

RUN apt-get install -y \
		libpq-dev

RUN docker-php-ext-configure zip --with-zlib-dir="/usr" && docker-php-ext-install zip

WORKDIR /tmp

# Upgrade ICU to 60.1
RUN curl -sS -o /tmp/icu.tar.gz -L http://download.icu-project.org/files/icu4c/60.1/icu4c-58_2-src.tgz && \
	tar -zxf /tmp/icu.tar.gz -C /tmp && \
	cd /tmp/icu/source && \
	./configure --prefix=/usr/local && \
	make && \
	make install

RUN docker-php-ext-configure intl --with-icu-dir=/usr/local && \
	docker-php-ext-install intl

RUN docker-php-ext-enable opcache

# Clean
RUN rm -rf /tmp/*

WORKDIR /var/www/symfony
