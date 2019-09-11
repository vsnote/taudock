FROM taudock/php-fpm:7.2-fpm

COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

COPY ./php.ini /usr/local/etc/php/conf.d

WORKDIR /var/www

# Install Git
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak2 && \
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ jessie main non-free contrib" >/etc/apt/sources.list
RUN apt-get update && apt-get install -y git

# Install Compose composer 
# update --ignore-platform-reqs
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
php composer-setup.php && \
php -r "unlink('composer-setup.php');" && \
mv composer.phar /usr/local/bin/composer

# Install Php Redis Extension
RUN printf "\n" | pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

# Install the mongodb extension
RUN pecl install mongodb && \
docker-php-ext-enable mongodb 