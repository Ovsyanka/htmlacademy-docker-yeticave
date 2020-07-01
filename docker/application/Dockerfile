FROM php:7.2-fpm-alpine

# Для установки composer пакетов может потребоваться GITHUB_TOKEN, если будет использоваться GitHub API. Это не
# обязательный аргумент, так как авторизация может и не потребоваться, это зависит от характера и частоты запросов.
ARG GITHUB_TOKEN

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

# Проверяю, есть ли токен и если есть добавляю его в настройки композера.
RUN if [ -n "$GITHUB_TOKEN" ]; then composer config -g github-oauth.github.com "$GITHUB_TOKEN"; fi

# Плагин для параллелизации (тем самым ускорения) установки пакетов.
RUN composer global require hirak/prestissimo --prefer-dist --no-scripts

WORKDIR /var/www/html
