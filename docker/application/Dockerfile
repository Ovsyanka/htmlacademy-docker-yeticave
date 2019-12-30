FROM php:7.1-fpm-alpine

# xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
  && pecl install xdebug-2.5.0 \
  && docker-php-ext-enable xdebug

# mysqli
RUN docker-php-ext-install mysqli

# Юзер, под которым работает php должен иметь права на запись в эту директорию
# Располагаю ее вне /var/www/html, чтобы не было путаницы - в html биндится директория с кодом.
# RUN mkdir /var/www/uploads && chown www-data:www-data /var/www/uploads
# Чтобы не менять php код проекта, который предполагает, что uploads будет там же где исходный код - решил оставить ее
# там. Вместо uploads в директории с кодом будет использоваться именованный volume (см. docker-compose.yml)
RUN mkdir /var/www/html/uploads && chown www-data:www-data /var/www/html/uploads
