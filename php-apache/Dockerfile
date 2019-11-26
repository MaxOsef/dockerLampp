ARG PHP_TAG

FROM php:${PHP_TAG}-apache

ARG WORKDIR
ARG WEB_USER
ARG WEB_GROUP

# PHP configuration
COPY php.ini /usr/local/etc/php/php.ini

# Librairies and extension installation
RUN apt-get update \
	&& apt-get install -y libmcrypt-dev \
		libjpeg62-turbo-dev \
		libpcre3-dev \
		libpng-dev \
		libfreetype6-dev \
		libxml2-dev \
		libicu-dev \
		libzip-dev \
		default-mysql-client \
		wget \
		unzip \
        git \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install iconv intl pdo_mysql mbstring soap gd zip

RUN docker-php-source extract \
    && if [ -d "/usr/src/php/ext/mysql" ]; then docker-php-ext-install mysql; fi \
    && if [ -d "/usr/src/php/ext/mcrypt" ]; then docker-php-ext-install mcrypt; fi \
    && if [ -d "/usr/src/php/ext/opcache" ]; then docker-php-ext-install opcache; fi \
    && docker-php-source delete

RUN a2enmod rewrite

# Adding vhosts configurations and enable them
COPY default.conf /etc/apache2/sites-available/
COPY five.devel.conf /etc/apache2/sites-available/
COPY six.devel.conf /etc/apache2/sites-available/
COPY seven.devel.conf /etc/apache2/sites-available/
COPY seven50.devel.conf /etc/apache2/sites-available/
COPY seven60.devel.conf /etc/apache2/sites-available/
COPY tbone.devel.conf /etc/apache2/sites-available/
COPY wpfive.devel.conf /etc/apache2/sites-available/

RUN ln -s /etc/apache2/sites-available/default.conf /etc/apache2/sites-enabled/default..conf
RUN ln -s /etc/apache2/sites-available/five.devel.conf /etc/apache2/sites-enabled/five.devel.conf
RUN ln -s /etc/apache2/sites-available/six.devel.conf /etc/apache2/sites-enabled/six.devel.conf
RUN ln -s /etc/apache2/sites-available/seven.devel.conf /etc/apache2/sites-enabled/seven.devel.conf
RUN ln -s /etc/apache2/sites-available/seven50.devel.conf /etc/apache2/sites-enabled/seven50.devel.conf
RUN ln -s /etc/apache2/sites-available/seven60.devel.conf /etc/apache2/sites-enabled/seven60.devel.conf
RUN ln -s /etc/apache2/sites-available/tbone.devel.conf /etc/apache2/sites-enabled/tbone.devel.conf
RUN ln -s /etc/apache2/sites-available/wpfive.devel.conf /etc/apache2/sites-enabled/wpfive.devel.conf

# Handling permissions
RUN usermod -u 1000 ${WEB_USER} \
    && groupmod -g 1000 ${WEB_GROUP} \
    && chgrp -R ${WEB_GROUP} /etc/apache2 \
    && chown -R ${WEB_USER}:${WEB_GROUP} ${WORKDIR} \
    && chmod ug+s ${WORKDIR}
