FROM php:5.6-apache
MAINTAINER Aman jain "jainaman23@gmail.com"

# Update SYstem
RUN apt-get update

# Install Composer
ENV COMPOSER_VERSION 1.2.3
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION}

# Install Required Packages
RUN apt-get install -y \
	nano \
  vim \
 	unzip \
  git \
  openssh-server \
  libbz2-dev \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libpng12-dev \
  libpq-dev \
  mysql-client \
  php-pear \
  && rm -r /var/lib/apt/lists/* \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr \
  && docker-php-ext-install bcmath bz2 calendar gd mbstring mcrypt pdo pdo_mysql pdo_pgsql zip

# Copy php.ini for fpm.
COPY ./php/php.ini /usr/local/etc/php/

# Enable Rewrite Module
RUN a2enmod rewrite

# Install 8.x branch as a git clone. Great for contributing back to Drush project.
RUN composer global require drush/drush:8.x-dev

# Install Drupal Console
RUN curl -sS https://getcomposer.org/installer | php

# Move to a globally accessing path on your system and apply it executable permissions
RUN mv composer.phar /usr/local/bin/composer.phar
RUN alias composer='/usr/local/bin/composer.phar'
WORKDIR /var/www/html
RUN composer require drupal/console:~1.0 --prefer-dist --optimize-autoloader
RUN curl https://drupalconsole.com/installer -L -o drupal.phar
RUN mv drupal.phar /usr/local/bin/drupal
RUN chmod +x /usr/local/bin/drupal

# Create Drupal Project
# RUN composer create-project drupal-composer/drupal-project:~8.0 . --stability dev --no-interaction
