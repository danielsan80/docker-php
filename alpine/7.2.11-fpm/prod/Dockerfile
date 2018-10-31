FROM matiux/php:7.2.11-fpm-alpine-base

ARG APP_ENV=prod

ARG COMPOSER_FLAGS="--no-dev --prefer-dist --optimize-autoloader --classmap-authoritative --no-interaction --no-suggest --no-progress --ansi"

COPY --chown=utente:www-data composer.* ./
RUN composer install $COMPOSER_FLAGS --no-scripts --no-autoloader \
    && mkdir var

# COPY --chown=utente:www-data web/app.php web/favicon.ico web/
# COPY --chown=utente:www-data bin/console bin/
# COPY --chown=utente:www-data app app
# COPY --chown=utente:www-data web/build/manifest.* web/build/
# COPY --chown=utente:www-data src srciproute2iproute2iproute2