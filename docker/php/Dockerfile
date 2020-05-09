FROM php:7.4-fpm-buster
LABEL maintainer="Daiki Domon <daiki.domon@todoonada.co.jp>"

# For socket connection
RUN addgroup --gid 101 nginx && \
  useradd -g nginx -d /home/nginx -s /bin/bash -u 101 nginx && \
  mkdir -p /var/run/php-fpm && chown nginx:nginx /var/run/php-fpm

# SHELL ["/bin/ash", "-oeux", "pipefail", "-c"]

# tinker(psysh)
ARG PSYSH_DIR=/usr/local/share/psysh
ARG PSYSH_PHP_MANUAL=$PSYSH_DIR/php_manual.sqlite
ARG PHP_MANUAL_URL=http://psysh.org/manual/ja/php_manual.sqlite

# timezone
ARG TZ

# composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer

RUN apt-get update && \
  apt-get install -y \
    autoconf \
    gcc \
    g++ \
    make \
    tzdata \
    git \
    libicu-dev \
    libzip-dev \
    libonig-dev \
    zip \
    unzip \
    vim && \
  cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
  echo ${TZ} > /etc/timezone && \
  pecl install xdebug && \
  git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis && \
  docker-php-ext-install intl pdo_mysql mbstring zip bcmath redis && \
  docker-php-ext-enable xdebug && \
  mkdir $PSYSH_DIR && curl $PHP_MANUAL_URL -o $PSYSH_PHP_MANUAL && \
  curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
  composer config -g process-timeout 3600 && \
  composer config -g repos.packagist composer https://packagist.jp && \
  composer global require hirak/prestissimo