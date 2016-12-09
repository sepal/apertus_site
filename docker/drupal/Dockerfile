FROM php:7.1-apache

ENV VIRTUAL_HOST="localhost" \
    XDEBUG_REMOTE_HOST="" \
    XDEBUG_REMOTE_AUTOSTART="Off" \
    ADMIN_EMAIL="admin@localhost" \
    DOCROOT="/var/www/web" \
    DB_HOST="database" \
    DB_PORT="3306" \
    DB_USER="" \
    DB_PASSWORD="" \
    DB_DATABASE="database" \
    COMPOSER_ALLOW_SUPERUSER=1

# Install basic cli tools and required packages.
RUN apt-get update \
  && apt-get install -y ssmtp git curl mysql-client rsync vim libpng12-dev libjpeg-dev libpq-dev \
  && rm -rf /var/lib/apt/lists/* \
  && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
  && docker-php-ext-install gd bcmath mbstring opcache pdo pdo_mysql pdo_pgsql zip \
  && pecl install xdebug \
  && a2enmod rewrite

# Install composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Install drush.
RUN composer global require drush/drush \
  && ln -s /root/.composer/vendor/bin/drush /usr/bin/drush

# Place custom apache config.
COPY ./docker/drupal/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN mkdir -p /var/www/private && mkdir -p /var/www/web/sites/default/files

COPY ./web/ /var/www/web/
COPY ./vendor /var/www/vendor
RUN chown -R www-data:www-data /var/www/vendor \
  && chown -R www-data:www-data /var/www/web \
  && chown www-data:www-data /var/www/private \

WORKDIR /var/www/web
