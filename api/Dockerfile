FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
  git zip unzip libpng-dev \
  libzip-dev default-mysql-client

RUN docker-php-ext-install pdo pdo_mysql zip gd

RUN a2enmod rewrite

WORKDIR /var/www/html

COPY . /var/www/html

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN COMPOSER_ALLOW_SUPERUSER=1 composer install --no-scripts --no-autoloader

EXPOSE 80

RUN sed -i 's!/var/www/html!/var/www/html/public!g' \
  /etc/apache2/sites-available/000-default.conf

RUN echo "<Directory /var/www/html/public>" > /etc/apache2/conf-available/symfony.conf \
  && echo "    AllowOverride All" >> /etc/apache2/conf-available/symfony.conf \
  && echo "</Directory>" >> /etc/apache2/conf-available/symfony.conf \
  && a2enconf symfony

CMD ["apache2-foreground"]

