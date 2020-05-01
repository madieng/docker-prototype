FROM php:7.4-apache

# COMPOSER
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"\
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"\
    && php composer-setup.php\
    && mv composer.phar /usr/local/bin/composer\
    && php -r "unlink('composer-setup.php');"

# PACKAGES

RUN apt-get update && apt-get install -y zlib1g-dev libicu-dev g++ wget curl git zip unzip
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl

# PHP extensions
RUN docker-php-ext-install pdo mysqli opcache
ADD config/php/php.ini /usr/local/etc/php/conf.d/php.ini

# Apache
ADD errors /errors
ADD config/apache/vhost.conf /etc/apache2/sites-available/000-default.conf
ADD config/apache/apache.conf /etc/apache2/conf-available/z-app.conf
RUN a2enconf z-app

# # Activation d'Apache mod_rewrite
# RUN a2enmod rewrite

# # On configure un vhost, pour ne pas avoir de public/ ou web/ dans l'URL
# COPY config/default-vhost.conf /etc/apache2/sites-available/default-vhost.conf
# # RUN a2dissite 000-default.conf
# # RUN a2ensite default-vhost.conf

# Cleaning
# RUN rm -rf /etc/apache2/sites-available/* && rm -rf /etc/apache2/sites-enabled/*
# RUN service apache2 restart

# # Conf Apache2
# RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
# COPY config/apache/default-vhost.conf /etc/apache2/sites-available/default-vhost.conf
# RUN usermod -u 1000 www-data
# RUN a2ensite default-vhost.conf
# RUN service apache2 restart

# SYMFONY-CLI
RUN wget https://get.symfony.com/cli/installer -O - | bash\
 && mv /root/.symfony/bin/symfony /usr/local/bin/symfony

RUN git config --global user.email "mbegue.m@gmail.com" && git config --global user.name "Madieng MBEGUERE"
