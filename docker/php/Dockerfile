FROM php:7.4-fpm-alpine

LABEL version=1.0.0 author="Winston Castillo" author.email=winz.git@gmail.com

# add extra tools
# freetype, libjpeg libpng libzip required by gd
RUN apk add --update --no-cache \
    $PHPIZE_DEPS \
    bash \
    vim \
    git \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libzip-dev \
    libxml2-dev \
    icu-dev \
    imagemagick \
	imagemagick-libs \
	imagemagick-dev \
	php7-imagick \
	php7-pcntl \
	php7-soap

# gd
RUN docker-php-ext-configure gd --with-jpeg --with-freetype \
    && docker-php-ext-install gd

# intl
RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl

# db connection
RUN docker-php-ext-install pdo_mysql \
                           soap \
                           zip \
                           opcache

# enable enable which myql library to use
RUN docker-php-ext-enable pdo_mysql

# enable imagick
RUN printf "\n" | pecl install \
		imagick && \
		docker-php-ext-enable --ini-name 20-imagick.ini imagick


## composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
# RUN composer self-update 1.9.0
RUN composer self-update

COPY ./config/opcache.ini /usr/local/etc/php
COPY ./config/php.ini /usr/local/etc/php

WORKDIR /var/www/html


EXPOSE 9000

CMD ["php-fpm"]