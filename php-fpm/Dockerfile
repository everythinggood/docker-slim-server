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

FROM laradock/php-fpm:2.0-71

MAINTAINER hong <ouranoshong@outlook.com>

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

#####################################
# xDebug:
#####################################
ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    pecl install xdebug && \
    docker-php-ext-enable xdebug \
;fi

COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#####################################
# APCU:
#####################################
ARG INSTALL_APCU=false
RUN if [ ${INSTALL_APCU} = true ]; then \
    pecl install APCu && \
    docker-php-ext-enable apcu \
;fi

COPY ./apcu.ini /usr/local/etc/php/conf.d/apcu.ini

#####################################
# MongoDB:
#####################################
ARG INSTALL_MONGO=false
RUN if [ ${INSTALL_MONGO} = true ]; then \
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
;fi

#####################################
# ZipArchive:
#####################################
ARG INSTALL_ZIP_ARCHIVE=false
RUN if [ ${INSTALL_ZIP_ARCHIVE} = true ]; then \
    docker-php-ext-install zip \
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

#####################################
# ImageMagick:
#####################################
USER root
ARG INSTALL_IMAGEMAGICK=false
RUN if [ ${INSTALL_IMAGEMAGICK} = true ]; then \
    apt-get update -y && \
    apt-get install -y libmagickwand-dev imagemagick && \
    apt-get clean && \
    pecl install imagick && \
    docker-php-ext-enable imagick \
;fi

#####################################
# Redis:
#####################################
ARG INSTALL_REDIS=true
RUN if [ ${INSTALL_REDIS} = true ]; then \
    curl -L -o /tmp/phpredis.tar.gz "https://github.com/phpredis/phpredis/archive/3.1.6.tar.gz"\
    && mkdir -p phpredis \
    && tar -C phpredis -zxvf /tmp/phpredis.tar.gz --strip 1 \
    && ( \
    cd phpredis \
    && phpize \
    && ./configure \
    && make -j$(nproc) \
     && make install \
    ) \
    && rm -r phpredis \
    && rm /tmp/phpredis.tar.gz \
    && docker-php-ext-enable redis \
;fi

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

ADD ./slim.ini /usr/local/etc/php/conf.d
ADD ./slim.pool.conf /usr/local/etc/php-fpm.d/

RUN usermod -u 1000 www-data

WORKDIR /var/www/slim

CMD ["php-fpm"]

EXPOSE 9000
