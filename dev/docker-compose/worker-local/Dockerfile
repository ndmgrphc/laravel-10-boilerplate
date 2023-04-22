FROM php:8.2-alpine3.16

ARG queueTimeout
ARG queueTries
ARG queues

ENV QUEUE_TIMEOUT=${queueTimeout}
ENV QUEUE_TRIES=${queueTries}
ENV QUEUES=${queues}

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

#RUN docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-enable pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

WORKDIR /var/www

# test: php artisan queue:work redis --queue=default --once
CMD php artisan queue:work redis --queue=${QUEUES} --timeout=${QUEUE_TIMEOUT} --tries={$QUEUE_TRIES}
