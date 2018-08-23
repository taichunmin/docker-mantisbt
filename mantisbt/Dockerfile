FROM php:7.1-apache
LABEL maintainer="taichunmin <taichunmin@gmail.com>"

ENV MANTIS_VER=2.16.0

RUN set -eux; \
    # 抓取 mantisbt
    MANTIS_URL="https://downloads.sourceforge.net/project/mantisbt/mantis-stable/${MANTIS_VER}/mantisbt-${MANTIS_VER}.tar.gz"; \
    if ! curl -fL -o mantisbt.tgz "${MANTIS_URL}"; then \
        echo >&2 "error: failed to download '${MANTIS_URL}'"; \
        exit 1; \
    fi; \
    tar -xz --strip-components=1 -f mantisbt.tgz; \
    rm mantisbt.tgz; \
    chown -R www-data:www-data .; \
    # apt-get
    apt-get update; \
    apt-get install -y --no-install-recommends \
        libjpeg-dev \
        libpng-dev \
        libpq-dev \
        libxml2-dev \
    ; \
    rm -rf /var/lib/apt/lists/*; \
    # 安裝 PHP 的模組
    docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
    docker-php-ext-install \
        gd \
        mbstring \
        mysqli \
        pgsql \
        soap \
    ; \
    # apache
    a2enmod rewrite; \
    # 時區
    ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime; \
    echo 'date.timezone = "Asia/Taipei"' > /usr/local/etc/php/php.ini;