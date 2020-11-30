FROM php:7.4-apache-buster
LABEL version="0.1.0"

ENV PHALCON_VERSION=4.1.0

# Install base PHP extensions.
RUN apt-get update \
    && apt-get install -y \
      # for php zip extension
      libzip-dev \
      # for phalcon https://docs.phalcon.io/4.0/en/installation#debian
      libpcre3-dev \
      # for mbstring https://github.com/docker-library/php/issues/880
      libonig-dev \
    && docker-php-ext-install zip \
    && docker-php-ext-install mbstring

# https://pecl.php.net/package/phalcon
RUN pecl install phalcon-${PHALCON_VERSION} \
    && docker-php-ext-enable phalcon

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
