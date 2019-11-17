FROM php:7.1-fpm-alpine

# xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
  && pecl install xdebug-2.5.0 \
  && docker-php-ext-enable xdebug

# mysqli
RUN docker-php-ext-install mysqli
