FROM php:7.2-fpm-alpine

# xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
  && pecl install xdebug-2.9.0 \
  && docker-php-ext-enable xdebug

# mysqli
RUN docker-php-ext-install mysqli

# Устанавливаю composer
RUN apk add --no-cache composer
# Альтернатива
# COPY --from=composer /usr/bin/composer /usr/bin/composer

# Отключаю варнинг насчет запуска композера от имени root. Чтобы глаза не мозолил. Это нормально запускать под рутом в
# контейнере.
ENV COMPOSER_ALLOW_SUPERUSER=1

# Таким образом я кэш отключать не могу, так как, похоже, prestissimo его использует. Вместо этого можно просто его
# удалять в конце. Но в данном случае я этого делать не буду, так как образ разработческий и кэш будет полезен.
# ENV COMPOSER_CACHE_DIR=/dev/null

# Плагин для параллелизации (тем самым ускорения) установки пакетов.
RUN composer global require hirak/prestissimo --prefer-dist --no-scripts

WORKDIR /var/www/html

# Юзер, под которым работает php должен иметь права на запись в эту директорию
# Располагаю ее вне /var/www/html, чтобы не было путаницы - в html биндится директория с кодом.
# RUN mkdir /var/www/uploads && chown www-data:www-data /var/www/uploads
# Чтобы не менять php код проекта, который предполагает, что uploads будет там же где исходный код - решил оставить ее
# там. Вместо uploads в директории с кодом будет использоваться именованный volume (см. docker-compose.yml)
RUN mkdir /var/www/html/uploads && chown www-data:www-data /var/www/html/uploads
