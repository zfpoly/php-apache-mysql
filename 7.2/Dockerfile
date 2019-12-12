FROM php:7.2-apache

RUN buildDeps="libpq-dev libzip-dev libicu-dev libfreetype6-dev libmagickwand-6.q16-dev" && \
    apt-get update && \
    apt-get install -y $buildDeps --no-install-recommends

RUN ln -s /usr/lib/x86_64-linux-gnu/ImageMagick-6.8.9/bin-Q16/MagickWand-config /usr/bin
RUN pecl install imagick && \
    echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install \
    opcache \
    bcmath \
    bz2 \
    calendar \
    exif \
    gettext \
    iconv \
    pcntl \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    sockets \
    shmop \
    intl \
    zip \
    && pecl install apcu \
    && docker-php-ext-enable apcu

RUN apt-get update \
    && apt-get install -y libwebp-dev libjpeg62-turbo-dev libpng-dev libxpm-dev libfreetype6-dev

# Install gd
RUN docker-php-ext-configure gd --with-gd --with-webp-dir --with-jpeg-dir \
    --with-png-dir --with-zlib-dir --with-xpm-dir --with-freetype-dir

RUN docker-php-ext-install gd

RUN pecl install xdebug-2.8.1 && docker-php-ext-enable xdebug
#configuration option "php_ini" is not set to php.ini location
#You should add "zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20180731/xdebug.so" to php.ini

COPY ./install-mysql.sh /usr/local/bin/install-mysql
RUN chmod +x /usr/local/bin/install-mysql
RUN /usr/local/bin/install-mysql

RUN apt-get install -y wget
COPY ./install-composer.sh /usr/local/bin/install-composer
RUN chmod +x /usr/local/bin/install-composer
RUN /usr/local/bin/install-composer
RUN mv composer.phar /usr/local/bin/composer

RUN apt-get install pv

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - && apt-get install -y nodejs

RUN npm -v

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt update && apt install --no-install-recommends yarn

RUN a2enmod ssl && a2enmod rewrite

RUN apt-get install -y git

RUN git config --global core.filemode false

WORKDIR /var/www/html
EXPOSE 80 443