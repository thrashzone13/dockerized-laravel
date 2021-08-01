ARG PHP_VERSION

FROM php:${PHP_VERSION}-fpm

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    apt-utils \
    exif \
    libzip-dev \
    libcurl4-openssl-dev \
    pkg-config \
    libssl-dev \
    sendmail

RUN docker-php-ext-install pdo pdo_mysql mysqli gd

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Start as root
USER root
###########################################################################
# non-root user:
###########################################################################
# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1001
ENV PUID ${PUID}
ARG PGID=1001
ENV PGID ${PGID}
ARG COMPOSE_PROJECT_NAME

# always run apt update when start and after add new source list, then clean up at end.
RUN set -xe; \
groupadd -g ${PGID} ${COMPOSE_PROJECT_NAME} \
&& useradd -u ${PUID} -g ${COMPOSE_PROJECT_NAME} -m ${COMPOSE_PROJECT_NAME}\
&& usermod -p "*" ${COMPOSE_PROJECT_NAME} -s /bin/bash \
&& groupadd docker \
&& usermod -aG docker ${COMPOSE_PROJECT_NAME}

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set the timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo "${TIMEZONE}" > /etc/timezone

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

USER www

EXPOSE 9000
EXPOSE 5000