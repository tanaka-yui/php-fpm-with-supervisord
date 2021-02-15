FROM php:7.4.13-fpm

RUN set -ex; \
  \
  apt-get update; \
  apt-get install -y \
    libzip-dev \
    libicu-dev \
    libpng-dev \
    libxml2-dev \
    zlib1g-dev \
    unzip \
    nginx \
    supervisor \
  ; \
  rm -rf /var/lib/apt/lists/*; \
  \
  docker-php-ext-install gd mysqli opcache xmlrpc intl soap zip

RUN mkdir -p /run/php/

RUN { \
    echo '[www]'; \
    echo 'user = www-data'; \
    echo 'group = www-data'; \
    echo 'listen = /run/php/php7.4-fpm.sock'; \
    echo 'listen.owner = www-data'; \
    echo 'listen.group = www-data'; \
    echo 'listen.mode = 0666'; \
    echo 'clear_env = no'; \
    echo 'access.log = /dev/null'; \
  } > /usr/local/etc/php-fpm.d/zz-docker.conf

COPY ./src/etc/php/7.4/fpm/php.ini /usr/local/etc/php/php.ini
COPY ./src/etc/nginx/sites-available/default /etc/nginx/sites-available/default
COPY ./docker/app/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

COPY ./dist /var/www/
RUN chown -R www-data:www-data /var/www/html/

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
