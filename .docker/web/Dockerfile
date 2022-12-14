FROM php:8.1-apache

USER root

# Set working directory
WORKDIR /var/www/html

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip 

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd opcache

RUN pecl install apfd  && docker-php-ext-enable apfd

## Copy configuration php
COPY ./php/conf.d/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# Copy configuration vhost apache
COPY ./apache/vhost.conf /etc/apache2/sites-available/000-default.conf

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN chown -R 777 /var/www/html \
    && a2enmod rewrite

RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
