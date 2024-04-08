# Importe l'image de base PHP 8.3 avec Apache
FROM php:8.3-apache

# Met à jour la liste des paquets et installe les dépendances nécessaires
RUN apt-get update && \
apt-get install -y --no-install-recommends \
zlib1g-dev libxml2-dev librabbitmq-dev less git openssh-client unzip vim libpng-dev curl libzip-dev locales apt-utils git libicu-dev g++ libpng-dev libxml2-dev libzip-dev libonig-dev libxslt-dev gnupg nano;

# Installe les extensions PHP nécessaires
RUN docker-php-ext-install pdo pdo_mysql mysqli sockets zip soap gd intl calendar dom mbstring xsl

# Installe des extensions PHP supplémentaires à l'aide de PECL
RUN pecl install apcu amqp mailparse xdebug

# Active les extensions PHP installées
RUN docker-php-ext-enable apcu amqp mailparse xdebug pdo_mysql gd opcache intl zip calendar dom mbstring xsl
RUN docker-php-ext-configure intl

# Télécharge et installe Composer
RUN curl --location --output /usr/local/bin/composer https://getcomposer.org/composer.phar && chmod +x /usr/local/bin/composer

# Met à jour la liste des paquets et installe Node.js et npm
RUN apt-get update && apt-get install -y \
    software-properties-common \
    nodejs \
    npm

# Affiche la version de Node.js installée
RUN echo node -v

# Met à jour npm, installe 'n' et 'yarn', et met à jour Node.js à la dernière version
RUN npm install npm@9 -g && \
    npm install n -g && \
    npm install yarn -g && \
    n latest

# Active le module 'rewrite' d'Apache
RUN a2enmod rewrite

# Définit le répertoire de travail dans le conteneur
WORKDIR /var/www/html

# Copie le fichier de configuration Apache dans le conteneur
COPY docker/php/000-default.conf /etc/apache2/sites-available/000-default.conf

# Ajoute le chemin des binaires de Composer au PATH
RUN PATH=$PATH:/var/www/html/vendor/bin:bin
