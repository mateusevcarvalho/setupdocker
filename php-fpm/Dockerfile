ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    cron \
    libpng-dev \
    libonig-dev \
    zip \
    nano \
    unzip \
    libzip-dev \
    libcurl4 \
    libcurl4-openssl-dev \
    pkg-config \
    libicu-dev \
    libssl-dev \
    zlib1g-dev \
    libxml2 \
    libxml2-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

#Install zip
RUN apt-get install -y \
    libzip-dev \
    zip \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath mysqli intl gd

ARG PHP_INSTALL_XDEBUG
RUN if [ ${PHP_INSTALL_XDEBUG} = true ]; then \
        #Install xdebug
        pecl install xdebug && docker-php-ext-enable xdebug \
    ;fi

ARG PHP_INSTALL_MONGO
RUN if [ ${PHP_INSTALL_MONGO} = true ]; then \
        #Install mongodb
        pecl install mongodb && docker-php-ext-enable mongodb \
    ;fi

ARG PHP_INSTALL_SOAP
RUN if [ ${PHP_INSTALL_SOAP} = true ]; then \
        #Install soap
        docker-php-ext-install soap \
    ;fi


# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

USER root

CMD ["php-fpm"]

EXPOSE 9000
