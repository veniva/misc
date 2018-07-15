FROM php:apache

LABEL author="Ventsislav Ivanov"

COPY .docker/php/dev/php.ini /usr/local/etc/php/

# copy a vhost
COPY .docker/etc/apache2/sites-available/some-site.conf        /etc/apache2/sites-available/

RUN a2enmod rewrite headers ssl
RUN a2dissite default-ssl.conf 000-default.conf
RUN a2ensite some-site.conf 

# Install some debian packages

# Install GD library required packages on Debian
## libfreetype6-dev libjpeg-turbo libpng12-dev required by PHP's GD
### libfreetype6-dev may also be specified as freetype-dev see https://github.com/docker-library/php/issues/225
RUN apt-get update -y && apt-get install -y libpng-dev libjpeg62-turbo-dev libfreetype6-dev

## zlib1g-dev is required by the php's zip extension
## sendmail is probably required to send emails ??
RUN  apt-get install -y zlib1g-dev sendmail  libmcrypt-dev

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN docker-php-ext-install pdo_mysql mysqli mbstring zip

# Enable for local development
RUN pecl install xdebug-2.6.0 && docker-php-ext-enable xdebug


# Create the vhost & log dirs
RUN mkdir /var/www/some-site/        && mkdir /var/log/apache2/some-site/

# Change the working directory
WORKDIR /var/www