FROM hyperf/hyperf:latest
LABEL maintainer="五五 <15093565100@163.com>" version="1.0" license="MIT"
ARG timezone

ENV TIMEZONE=${timezone:-"Asia/Shanghai"} \
    APP_ENV=dev \
    SCAN_CACHEABLE=(true) \
    PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc libc-dev make php7-dev php7-pear pkgconf re2c pcre-dev zlib-dev libtool automake"
RUN set -ex \
    && apk update \
    && apk add --no-cache libstdc++ openssl git bash \
    && apk add --no-cache $PHPIZE_DEPS libaio-dev openssl-dev \
    # install composer
    && cd /tmp \
    && wget https://mirrors.aliyun.com/composer/composer.phar \
    && chmod u+x composer.phar && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer \
    && php -v \
    && php -m \
    && php --ri swoole \
    #  ---------- some config ----------
    && cd /etc/php7 \
    # - config PHP
    && { \
        echo "upload_max_filesize=100M"; \
        echo "post_max_size=108M"; \
        echo "memory_limit=1024M"; \
        echo "date.timezone=${TIMEZONE}"; \
    } | tee conf.d/99_overrides.ini \
    # - config timezone
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone \
    # ---------- clear works ----------
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"