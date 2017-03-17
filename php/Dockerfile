FROM php:7.1-fpm

MAINTAINER Alexander van Zyl <vanzyl.alex@gmail.com>

# Install require PHP extensions and their dependencies.
# We also install Phpredis, xdebug and composer. 
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libpq-dev \
        libmemcached-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        curl \
        git \
    && docker-php-ext-install \
        mcrypt \
        bcmath \
        pdo_mysql \
        pdo_pgsql \
        gd \
        zip \
    && pecl install redis-3.1.1 \
    && pecl install xdebug-2.5.0 \
    && docker-php-ext-enable redis xdebug \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir -p /run/php \
    && mkdir /.composer \
    && chown -R 1000 /.composer \
    && apt-get autoclean \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV COMPOSER_HOME=/.composer

# Hack to change uid of 'www-data' to 1000
RUN usermod -u 1000 www-data

COPY laravel.ini /usr/local/etc/php/conf.d

EXPOSE 9000
CMD ["php-fpm"]