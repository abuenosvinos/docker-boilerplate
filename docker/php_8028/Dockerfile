FROM php:8.0.28-fpm

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install PHP extensions and PECL modules.
RUN buildDeps=" \
        default-libmysqlclient-dev \
    " \
    runtimeDeps=" \
        git \
        unzip \
        curl \
        librabbitmq-dev \
        libfreetype6-dev \
        libicu-dev \
        libjpeg-dev \
        libpng-dev \
        libonig-dev \
        libzip-dev \
    " \
    && apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y $buildDeps $runtimeDeps \
    && docker-php-ext-install mysqli iconv mbstring intl pdo_mysql zip \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install gd && docker-php-ext-enable gd \
    && docker-php-ext-install exif \
    && apt-get purge -y --auto-remove $buildDeps \
    && rm -r /var/lib/apt/lists/*

RUN pecl install amqp \
    && docker-php-ext-enable amqp

# create symlink for php8
RUN ln -sf /usr/bin/php8 /usr/bin/php

# Copy custom php configuration
COPY php.ini /usr/local/etc/php/

WORKDIR /var/www
