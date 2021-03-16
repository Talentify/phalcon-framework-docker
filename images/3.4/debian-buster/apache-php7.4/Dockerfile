FROM php:7.4-apache-buster
LABEL version="0.1.0"

ENV PHALCON_VERSION=3.4.5

# Install base PHP extensions.
RUN apt-get update && apt-get install -y \
        libzip-dev \
        && docker-php-ext-install zip \
        && docker-php-ext-install mbstring

# Build Phalcon
RUN cd /usr/src \
        && curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
        && cd cphalcon-${PHALCON_VERSION}/build \
        && sed -i -e 's/-march=native -mtune=native/-march=core-avx-i -mtune=core-avx-i/g' install \
        && ./install \
        && cd ../../ \
        && rm -r cphalcon-${PHALCON_VERSION} \
        && echo 'extension=phalcon.so' > "$PHP_INI_DIR/conf.d/ext-phalcon.ini"

COPY --from=composer:22sr/bin/composer /usr/bin/composer
