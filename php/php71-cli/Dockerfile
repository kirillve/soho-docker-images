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
        php7.1-redis \
        php-pear \
        unzip \
    && apt clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc /usr/share/man

COPY docker-php-entrypoint /usr/local/bin/
ENTRYPOINT ["docker-php-entrypoint"]

WORKDIR /app