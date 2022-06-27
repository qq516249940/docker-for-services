FROM php:7.4-fpm@sha256:08a0f6fa0f53cd163245949c55a4cc598f4160d4fd217b5406f2c5575752a8e3
LABEL author="chunk"

RUN set -eux; \
    echo "deb http://mirrors.aliyun.com/debian/ buster main non-free contrib \n \
      deb-src http://mirrors.aliyun.com/debian/ buster main non-free contrib \n \
      deb http://mirrors.aliyun.com/debian-security buster/updates main \n \
      deb-src http://mirrors.aliyun.com/debian-security buster/updates main \n \
      deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib \n \
      deb-src http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib \n \
      deb http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib \n \
      deb-src http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib" > /etc/apt/sources.list
# 更新安装依赖包和PHP核心拓展
RUN set -ex; \
	apt-get update; \
	apt-get install -y  \
		imagemagick \
		less \
		mariadb-client msmtp \
		libc-client-dev \
		libfreetype6-dev \
		libjpeg-dev \
		libjpeg62-turbo-dev \
		libkrb5-dev \
		libmagickwand-dev \
		libgraphicsmagick1-dev \
		libmcrypt-dev \
		libicu-dev \
		libmemcached-dev \
		libxml2-dev \
		libpng-dev \
		libzip-dev \
		libssl-dev \
		libgmp-dev \
		unzip \
		vim \
		zip 

RUN pecl install imagick; \
	pecl install memcached; \
	pecl install mcrypt; \
	pecl install redis; \
    pecl install apcu; \
    pecl install gmagick-2.0.6RC1; \
    pecl install timezonedb; \
	docker-php-ext-configure gd --with-freetype --with-jpeg; \
	docker-php-ext-configure zip; \
	docker-php-ext-install gd; \
	PHP_OPENSSL=yes docker-php-ext-configure imap --with-kerberos --with-imap-ssl; \
	docker-php-ext-install imap; \
	docker-php-ext-install mysqli; \
	docker-php-ext-install pdo_mysql; \
	docker-php-ext-install opcache; \
	docker-php-ext-install soap; \
	docker-php-ext-install intl; \
	docker-php-ext-install zip; \
	docker-php-ext-install exif; \
    docker-php-ext-install calendar; \
    docker-php-ext-install gmp; \
    docker-php-ext-install pcntl; \
    docker-php-ext-install shmop; \
    docker-php-ext-install sockets; \
    docker-php-ext-install sysvsem; \
    docker-php-ext-install sysvshm; \
	docker-php-ext-install bcmath; \
	docker-php-ext-enable imagick mcrypt redis timezonedb apcu; \
        echo "extension=memcached.so" >> /usr/local/etc/php/conf.d/memcached.ini; \
	echo "extension=gmagick.so" >> /usr/local/etc/php/conf.d/gmagick.ini; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*;

#        && usermod -u 1000 www-data\
#        && groupmod -g 1000 www-data
# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH
CMD ["php-fpm"]
