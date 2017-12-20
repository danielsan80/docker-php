FROM debian:stretch

RUN apt-get update && apt-get install -y apt-transport-https lsb-release ca-certificates wget gnupg curl aptitude \
      && curl https://packages.sury.org/php/apt.gpg | apt-key add - \
      && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list \
      && apt-get update && apt-get install -y software-properties-common \
      vim vim-gtk3 \
      php5.6-fpm php5.6-cli \
      php5.6-bcmath \
      php5.6-bz2 php5.6-cgi php5.6-cli \
      php5.6-common php5.6-curl php5.6-dev \
      php5.6-imap php5.6-json php5.6-mbstring \
      php5.6-mcrypt php5.6-mysql php5.6-opcache \
      php5.6-readline php5.6-sqlite3 php5.6-xml php5.6-zip \
      && apt-get remove --purge -y software-properties-common \
      && apt-get autoremove -y \
      && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 9000
# CMD ["/usr/local/sbin/php-fpm", "--nodaemonize"]