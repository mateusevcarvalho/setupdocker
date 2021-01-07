FROM php:7.0.29-apache

RUN apt-get update -yqq \
    && apt-get install -yqq --no-install-recommends \
    git \
    unzip \
    nano \
    vim \
    cron \
    libreadline-dev \
    && rm -rf /var/lib/apt/lists

# Enable PHP extensions
RUN docker-php-ext-install pdo_mysql mysqli mbstring

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

# Add cake and composer command to system path
ENV PATH="${PATH}:/var/www/html/lib/Cake/Console"
ENV PATH="${PATH}:/var/www/html/app/Vendor/bin"


ARG INSTALL_XDEBUG_CAKE=false
ENV INSTALL_XDEBUG_CAKE ${INSTALL_XDEBUG_CAKE}

RUN if [ ${INSTALL_XDEBUG_CAKE} = true ]; then \
    #Install xdebug
    pecl install xdebug-2.5.5 && docker-php-ext-enable xdebug \
;fi

# install GD and mcrypt
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

#Install zip
RUN apt-get install -y \
    libzip-dev \
    zip \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip

# Create tmp directory and make it writable by the web server
RUN mkdir -p \
    tmp/cache/models \
    tmp/cache/persistent \
    && chown -R :www-data \
    tmp \
    && chmod -R 770 \
    tmp

ADD ./apache/000-default.conf /etc/apache2/sites-available/

# Enable Apache modules and restart
RUN a2enmod rewrite \
    && service apache2 restart

EXPOSE 80
