FROM php:8.3-fpm-alpine3.19

RUN apk add --no-cache \
 libzip-dev \
 zip \
 libpng-dev \
 libxslt-dev \
 $PHPIZE_DEPS \
 freetype-dev \
 libjpeg-turbo-dev \
 rabbitmq-c-dev \
 mariadb-client \
 openssl \
 acl exiftool font-noto-emoji nodejs npm
#RUN npm install puppeteer --save
# Install the PHP pdo_mysql extention
RUN docker-php-ext-install pdo_mysql xsl
# exif
RUN docker-php-ext-configure exif
RUN docker-php-ext-install exif
RUN docker-php-ext-enable exif

# Install intl extension
RUN apk add --no-cache \
    icu-dev \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-enable intl \
    && rm -rf /tmp/*

# Install mbstring extension
RUN apk add --no-cache \
    oniguruma-dev \
    && docker-php-ext-install mbstring \
    && docker-php-ext-enable mbstring \
    && rm -rf /tmp/* \

# Install the PHP gd library
RUN docker-php-ext-configure gd && docker-php-ext-install gd

RUN docker-php-ext-install pdo_mysql gd zip bcmath
RUN docker-php-ext-enable opcache

# Install APCu extension
RUN set -eux; \
    apk --no-cache add --virtual .build-deps $PHPIZE_DEPS; \
    pecl install apcu; \
    docker-php-ext-enable apcu; \
    apk del .build-deps;

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

COPY php.ini $PHP_INI_DIR/conf.d/custom.ini
WORKDIR /var/www/html
CMD ["php-fpm"]
