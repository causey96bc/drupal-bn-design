#Thanks to https://github.com/docker-library/drupal/blob/master/8.7/fpm-alpine/Dockerfile
FROM php:7.3-fpm-alpine

# install the PHP extensions we need
# postgresql-dev is needed for https://bugs.alpinelinux.org/issues/3642
RUN set -eux; \
  \
  apk add --no-cache --virtual .build-deps \
    coreutils \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libzip-dev \
    postgresql-dev \
  ; \
  \
  docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include \
    --with-jpeg-dir=/usr/include \
    --with-png-dir=/usr/include \
  ; \
  \
  docker-php-ext-install -j "$(nproc)" \
    gd \
    opcache \
    pdo_mysql \
    pdo_pgsql \
    zip \
  ; \
  \
  runDeps="$( \
    scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
      | tr ',' '\n' \
      | sort -u \
      | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
  )"; \
  apk add --virtual .drupal-phpexts-rundeps $runDeps; \
  apk del .build-deps; \
  apk add bash mysql-client git patch; \
  curl -sS https://getcomposer.org/download/1.10.10/composer.phar -o /usr/local/bin/composer && chmod +x /usr/local/bin/composer

# You should place drupal in current dir using the following command ...
# composer create-project drupal-composer/drupal-project:8.x-dev drupal --no-interaction
# And then mount volume in docker-compose.yml like this ...
# php:
#   volumes:
#     - $PWD/drupal/web:/var/www/html
#     - $PWD/drupal/vendor:/var/www/vendor
