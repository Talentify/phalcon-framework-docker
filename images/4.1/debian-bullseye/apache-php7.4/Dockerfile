FROM php:7.4-apache-bullseye
LABEL version="0.1.0"

ENV PHALCON_VERSION=4.1.3

# Install base PHP extensions.
RUN apt-get update && apt-get install -y \
        libzip-dev \
	git \
        && docker-php-ext-install zip

# Build PSR https://github.com/jbboehr/php-psr
RUN git clone https://github.com/jbboehr/php-psr.git \ 
	&& cd php-psr \ 
	&& phpize \
	&& ./configure \
	&& make \
	&& make test \ 
	&& make install \
	&& cd ../ \
	&& rm -rf php-psr \
	&& echo 'extension=psr.so' > "$PHP_INI_DIR/conf.d/40-psr.ini"

# Download Zephir
RUN curl -sSL "https://github.com/zephir-lang/zephir/releases/download/0.16.0/zephir.phar" -o /usr/bin/zephir \
	&& chmod +x /usr/bin/zephir

# Build Phalcon
RUN git clone https://github.com/phalcon/cphalcon \
	&& cd cphalcon \
	&& git checkout "tags/v${PHALCON_VERSION}" \
        && zephir fullclean \
	&& zephir compile \
	&& cd ext \
	&& phpize \
	&& ./configure \
	&& make \
	&& make install \
        && cd ../../ \
        && rm -r cphalcon \
        && echo 'extension=phalcon.so' > "$PHP_INI_DIR/conf.d/50-phalcon.ini"

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
