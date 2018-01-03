FROM debian:jessie

ARG HOST_UID
ARG HOST_GID

ENV HOST_UID ${HOST_UID:-1000}
ENV HOST_GID ${HOST_GID:-1000}

RUN apt-get update && apt-get install -y \
   wget curl aptitude software-properties-common \
   supervisor acl \
   vim vim-gtk \
   php5-fpm \
   php5-apcu \
   php5-memcached memcached \
   php5-curl \
   php5-gd \
   php5-imagick \
   php5-mcrypt \
   php5-intl \
   php5-mysql \
   php5-xdebug \
   php5-cgi \
   php5-cli \
   php5-sqlite \
   && apt-get remove --purge -y software-properties-common \
   && apt-get autoremove -y \
   && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Configure PHP settings
RUN sed -i "s/display_errors = Off/display_errors = On/" /etc/php5/fpm/php.ini
RUN sed -i "s/display_errors = Off/display_errors = On/" /etc/php5/cli/php.ini

RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /etc/php5/fpm/php.ini
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /etc/php5/cli/php.ini

RUN sed -i -e 's/allow_url_fopen = Off/allow_url_fopen = On/g' /etc/php5/fpm/php.ini
#RUN sed -i -e 's/expose_php = On/expose_php = Off/g' /etc/php5/fpm/php.ini

RUN sed -i -e 's/listen = \/var\/run\/php5-fpm.sock/listen = 0.0.0.0:9000/g' /etc/php5/fpm/pool.d/www.conf \
    && sed -i -e 's/;daemonize = yes/daemonize = no/g' /etc/php5/fpm/php-fpm.conf

#Composer
RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
&& curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
&& php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
&& php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --snapshot \
&& rm -f /tmp/composer-setup.*

#Configuro Xdebug
COPY xdebug.ini /etc/php5/mods-available/xdebug.ini
RUN sed -i "s/xdebug\.remote_host=.*/xdebug\.remote_host="$(/sbin/ip route|awk '/default/ { print $3 }')"/" /etc/php5/mods-available/xdebug.ini
ENV PHP_IDE_CONFIG=serverName=application

#Gestione utente
RUN mkdir /home/utente
#Aggiungo all'utente il gruppo
RUN groupadd -g $HOST_GID utente
RUN useradd -u $HOST_UID --gid $HOST_GID -d /home/utente -s /bin/bash utente
RUN usermod -a -G www-data utente
RUN chown utente /home/utente

WORKDIR /var/www/app

RUN echo 'alias ll="ls -alFh"' >> /home/utente/.bashrc

##EXPOSE 9000

CMD ["/usr/bin/supervisord"]