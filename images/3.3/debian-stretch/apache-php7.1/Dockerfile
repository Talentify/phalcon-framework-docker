FROM php:7.1-apache-stretch
LABEL version="0.1.0"

ENV PHALCON_VERSION=3.3.2

# Install base PHP extensions.
RUN apt-get update && apt-get install -y \
        zlib1g-dev \
        && docker-php-ext-install zip \
        && docker-php-ext-install mbstring

RUN cd /usr/src \
        && curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
        && cd cphalcon-${PHALCON_VERSION}/build \
        && ./install \
        && cd ../../ \
        && rm -r cphalcon-${PHALCON_VERSION}

RUN echo 'extension=phalcon.so' > /usr/local/etc/php/conf.d/ext-phalcon.ini

COPY --from=composer:1.5 /usr/bin/composer /usr/bin/composer
