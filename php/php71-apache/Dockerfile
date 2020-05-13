FROM debian:buster-slim

ENV DEBIAN_FRONTEND noninteractive
RUN set -x \
    && apt update -qq \
    && apt -y dist-upgrade \
    && apt install -y wget apt-transport-https apt-utils gnupg2 \
    && echo "deb https://packages.sury.org/php/ buster main" > /etc/apt/sources.list.d/php.list \
    && wget --quiet -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
    && echo "deb http://apt.postgresql.org/pub/repos/apt/ buster-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && apt update \
    && apt install -y \
        postgresql-client-12 \
        php7.1-curl \
        php7.1-cli \
        php7.1-gd \
        php7.1-intl \
        php7.1-json \
        php7.1-pgsql \
        php7.1-mysqli \
        php7.1-xsl \
        php7.1-mbstring \
        php7.1-bcmath \
        php7.1-zip \
        php7.1-imagick \
        php7.1-redis \
        php-pear \
        unzip \
        apache2 \
        libapache2-mod-xsendfile \
        libapache2-mod-php7.1 \
    && apt clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc /usr/share/man \
    && a2enmod rewrite xsendfile php7.1 headers \
    && ln -sf /proc/1/fd/1 /var/log/apache2/access.log \
    && ln -sf /proc/1/fd/2 /var/log/apache2/error.log \
    && sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
    && sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf \
    && sed -i '/<Directory ${APACHE_DOCUMENT_ROOT}>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

EXPOSE 80
COPY docker-php-entrypoint /usr/local/bin/
COPY apache2-foreground /usr/local/bin/
ENTRYPOINT ["docker-php-entrypoint"]
CMD ["apache2-foreground"]

ENV APACHE_DOCUMENT_ROOT /app/public
WORKDIR /app