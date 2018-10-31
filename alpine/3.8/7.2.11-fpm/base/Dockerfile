FROM php:7.2.11-fpm-alpine3.8

RUN apk add --no-cache \
        git \
        icu-dev \
        libmemcached-libs \
        zlib-dev \
    && apk add --no-cache --virtual .ext-build-deps \
        gcc \
        g++ \
        make \
        libc-dev \
        autoconf \
        libmemcached-dev \
    && pecl install -o \
        memcached-3.0.4 \
    && docker-php-ext-enable \
        memcached \
    && docker-php-ext-install -j5 \
        #`-j` è come per make, per usare 5 thread, accelera la compilazione
        #la regola è che è sempre meglio averla più alta del numero di core a disposzione. La sfiga è che non
        #si può parametrizzare, o ti si invalida la cache della build perché Docker vede un'istruzione diversa
        #(se cambi il parametro)
        intl \
        opcache \
        pdo_mysql \
        zip \
        bcmath \
        json \
        mbstring \
    && apk del .ext-build-deps

WORKDIR /var/www/app

RUN adduser -D -u 1000 utente -G www-data \
    && mkdir -p /home/utente \
    && chown -R utente /home/utente \
    && chown -R utente /var/www/app \
 ## Composer
    && curl -sS https://getcomposer.org/installer | php -- \
        --install-dir=/usr/local/bin --filename=composer \
        --version 1.7.2

USER utente

ARG GITHUB_TOKEN

RUN composer config -g github-oauth.github.com $GITHUB_TOKEN \
    && composer global require \
        --no-suggest --no-interaction \
        hirak/prestissimo \
    && rm -rf ~/.composer/cache