
FROM php:8.3-cli-bookworm

# Herramientas y bibliotecas para extensiones PHP
RUN apt-get update && apt-get install -y --no-install-recommends \
    git unzip zip curl \
    libicu-dev libzip-dev libxml2-dev \
    libpng-dev libjpeg62-turbo-dev libfreetype6-dev \
    libonig-dev \
    libcurl4-openssl-dev pkg-config libssl-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j"$(nproc)" \
       bcmath \
       exif \
       gd \
       intl \
       mbstring \
       opcache \
       pcntl \
       pdo_mysql \
       zip \
       xml \
    && yes '' | pecl install --configureoptions='with-mongodb-ssl="openssl"' mongodb \
    && docker-php-ext-enable mongodb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Composer oficial
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 8000

CMD ["php", "-S", "0.0.0.0:8000", "-t", "public"]
