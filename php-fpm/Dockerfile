#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#
# To edit the 'php-fpm' base Image, visit its repository on Github
#    https://github.com/LaraDock/php-fpm
#
# To change its version, see the available Tags on the Docker Hub:
#    https://hub.docker.com/r/laradock/php-fpm/tags/
#

FROM php:5.6-cli

LABEL Mahmoud Zalt <mahmoud@zalt.me>

#
#--------------------------------------------------------------------------
# Mandatory Software's Installation
#--------------------------------------------------------------------------
#
# Mandatory Software's such as ("mcrypt", "pdo_mysql", "libssl-dev", ....)
# are installed on the base image 'laradock/php-fpm' image. If you want
# to add more Software's or remove existing one, you need to edit the
# base image (https://github.com/LaraDock/php-fpm).
#

#
#--------------------------------------------------------------------------
# Optional Software's Installation
#--------------------------------------------------------------------------
#
# Optional Software's will only be installed if you set them to `true`
# in the `docker-compose.yml` before the build.
# Example:
#   - INSTALL_ZIP_ARCHIVE=true
#

RUN apt-get update && pecl channel-update pecl.php.net

#####################################
# xDebug:
#####################################

ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    # Install the xdebug extension
    pecl install xdebug-2.5.5 && \
    docker-php-ext-enable xdebug \
;fi

# Copy xdebug configration for remote debugging
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#####################################
# Redis extension:
#####################################

ARG INSTALL_REDIS_EXTENSION=false
RUN if [ ${INSTALL_REDIS_EXTENSION} = true ]; then \
    # Install the Redis extension
    pecl install -o -f redis-2.2.8 \
    &&  rm -rf /tmp/pear \
    &&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini \
;fi

# Copy xdebug configration for remote debugging
# COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#####################################
# MongoDB:
#####################################

ARG INSTALL_MONGO=false
RUN if [ ${INSTALL_MONGO} = true ]; then \
    # Install the mongodb extension
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
;fi

#####################################
# ZipArchive:
#####################################

ARG INSTALL_ZIP_ARCHIVE=false
RUN if [ ${INSTALL_ZIP_ARCHIVE} = true ]; then \
    # Install the zip extension
    pecl install zip && \
    docker-php-ext-enable zip \
;fi

#####################################
# PHP Memcached:
#####################################

ARG INSTALL_MEMCACHED=false
RUN if [ ${INSTALL_MEMCACHED} = true ]; then \
    # Install the php memcached extension
    pecl install memcached && \
    docker-php-ext-enable memcached \
;fi

#####################################
# PHP Aerospike:
#####################################

ARG INSTALL_AEROSPIKE_EXTENSION=false
ENV INSTALL_AEROSPIKE_EXTENSION ${INSTALL_AEROSPIKE_EXTENSION}
# Copy aerospike configration for remote debugging
COPY ./aerospike.ini /usr/local/etc/php/conf.d/aerospike.ini
RUN if [ ${INSTALL_AEROSPIKE_EXTENSION} = true ]; then \
    # Install the php aerospike extension
    curl -L -o /tmp/aerospike-client-php.tar.gz "https://github.com/luciano-jr/aerospike-client-php/archive/master.tar.gz" \
    && mkdir -p aerospike-client-php \
    && tar -C aerospike-client-php -zxvf /tmp/aerospike-client-php.tar.gz --strip 1 \
    && ( \
        cd aerospike-client-php/src/aerospike \
        && phpize \
        && ./build.sh \
        && make install \
    ) \
    && rm /tmp/aerospike-client-php.tar.gz \
;fi

RUN if [ ${INSTALL_AEROSPIKE_EXTENSION} = false ]; then \
    rm /usr/local/etc/php/conf.d/aerospike.ini \
;fi

#####################################
# Opcache:
#####################################

ARG INSTALL_OPCACHE=false
RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache && \
    docker-php-ext-enable opcache \
;fi

COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini
# Copy opcache configration
RUN if [ ${INSTALL_OPCACHE} = false ]; then \
  rm /usr/local/etc/php/conf.d/opcache.ini \
;fi

#####################################
# Codeigniter Modifications:
#####################################

ARG CODEIGNITER=false
RUN if [ ${CODEIGNITER} = true ]; then \
    # Install Codeigniter PHP extentions requirements
     docker-php-ext-install mysqli && \
     docker-php-ext-install tokenizer \
;fi

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

ADD ./laravel.ini /usr/local/etc/php/conf.d
ADD ./laravel.pool.conf /usr/local/etc/php-fpm.d/

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

WORKDIR /var/www/tms

CMD ["php-fpm"]

RUN docker-php-ext-install mysqli

EXPOSE 9000
