FROM php:7.1-fpm
ARG TIMEZONE

MAINTAINER Roman Bylbas <roman.bylbas@gmail.com>

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    zlib1g-dev \
    autoconf \
    pkg-config \
    libssl-dev \
    libpq-dev

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install \
    pdo \
    pdo_pgsql \
    bcmath \
    zip

# Install mongo
RUN pecl install mongodb-1.2.2
RUN echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini

# Install redis
RUN pecl install -o -f redis
RUN rm -rf /tmp/pear
RUN docker-php-ext-enable redis

# Install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN echo 'alias console="php app/console"' >> ~/.bashrc

WORKDIR /var/www/symfony
