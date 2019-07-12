FROM php:7.2-apache-stretch
LABEL version="0.2.0"

ENV PHALCON_VERSION=3.3.2

# Install base PHP extensions.
RUN apt-get update && apt-get install -y \
        libzip-dev \
        && docker-php-ext-install zip \
        && docker-php-ext-install mbstring

# Build Phalcon
RUN cd /usr/src \
        && curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
        && cd cphalcon-${PHALCON_VERSION}/build \
        && sed -i -e 's/-march=native/-march=${GCC_ARCH}/g' install \
        && sed -i -e 's/-mtune=native/-mtune=${GCC_TUNE}/g' install \
        && ./install \
        && cd ../../ \
        && rm -r cphalcon-${PHALCON_VERSION} \
        && echo 'extension=phalcon.so' > "$PHP_INI_DIR/conf.d/ext-phalcon.ini"

COPY --from=composer:1.5 /usr/bin/composer /usr/bin/composer
