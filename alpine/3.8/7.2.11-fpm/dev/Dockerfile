FROM matiux/php:7.2.11-fpm-alpine3.8-base

USER root

RUN apk --no-cache add \
    nano \
    sudo \
    vim \
    wget \
    curl \
    zsh \   
    && apk add --no-cache --virtual .xdebug-build-deps $PHPIZE_DEPS \
    && pecl install -o xdebug \
    && docker-php-ext-enable xdebug \
    && apk del .xdebug-build-deps \
    && pecl clear-cache \
    && docker-php-source delete \
    && echo 'memory_limit = 512M' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini;

COPY conf/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#Xdebug
COPY conf/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
RUN sed -i "s/xdebug\.remote_host=.*/xdebug\.remote_host="$(/sbin/ip route|awk '/default/ { print $3 }')"/" /usr/local/etc/php/conf.d/xdebug.ini
ENV PHP_IDE_CONFIG=serverName=application

USER utente

COPY conf/custom_zshrc /home/utente
RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true \
    && echo 'source ~/custom_zshrc' >> /home/utente/.zshrc