FROM php:7.2-fpm

RUN groupmod -g 402 www-data
RUN usermod -u 401 www-data

RUN apt-get update \
    && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libz-dev \
        less \
        mysql-client \
        libmemcached11 \
        libmemcachedutil2 \
        libmemcached-dev \
        curl \
    && docker-php-ext-install -j$(nproc) \
        mysqli \
        pdo \
        pdo_mysql \
        zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && apt-get remove -y build-essential libz-dev libmemcached-dev \
    && apt-get autoremove -y \
    && apt-get clean

RUN curl https://getcomposer.org/download/$(curl -LSs https://api.github.com/repos/composer/composer/releases/latest | grep 'tag_name' | sed -e 's/.*: "//;s/".*//')/composer.phar > composer.phar \
    && chmod +x composer.phar \
    && mv composer.phar /usr/local/bin/composer \
    && curl https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar > wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

RUN curl -O https://wordpress.org/wordpress-${WORDPRESS_VERSION}.tar.gz && \
    tar -xvzf wordpress-${WORDPRESS_VERSION}.tar.gz -C /var/www/html && \
    rm wordpress-${WORDPRESS_VERSION}.tar.gz && \
    mv /var/www/html/wordpress/* /var/www/html/ && \
    rm -rf /var/www/html/wordpress && \
    rm -rf /var/www/html/wp-content/themes/* && \
    rm -rf /var/www/html/wp-content/plugins/* && \
    mkdir -p /var/www/html/wp-content/mu-plugins && \
    mkdir -p /var/www/html/vendor

#copy php.ini
COPY ./php.ini /usr/local/etc/php/php.ini


EXPOSE 9000

CMD ["php-fpm"]
