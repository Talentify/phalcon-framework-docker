FROM php:7.3-apache-stretch
LABEL version="0.2.0"

# https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html
ARG GCC_ARCH=native
# https://gcc.gnu.org/onlinedocs/gcc-4.5.3/gcc/i386-and-x86_002d64-Options.html
ARG GCC_TUNE=native
ENV PHALCON_VERSION=3.4.3

# Install base PHP extensions.
RUN apt-get update && apt-get install -y \
        libzip-dev \
        && docker-php-ext-install zip

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
