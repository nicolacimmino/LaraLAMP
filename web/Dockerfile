FROM php:7.1-apache

RUN apt-get update && apt-get -y install git curl php5-mcrypt php5-json zip
RUN /usr/sbin/a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer
RUN mkdir -p /var/www/laravel/storage 
RUN mkdir -p /var/www/laravel/bootstrap/cache
RUN mkdir -p /var/www/public
RUN chown www-data:www-data -R /var/www/laravel/storage /var/www/laravel/bootstrap/cache

# Change default website root to /var/www
RUN sed -i 's/DocumentRoot \/var\/www\/html\/DocumentRoot/\/var\/www\/public/' /etc/apache2/sites-enabled/000-default.conf

# Install ssh so we can use sftp from PHPStorm and avoid volumes
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

# Out of the box this is configured for root ssh without password
# PHPStorm doesn't have this option though, set root password to root
# and allow ssh with password.
# Also allow older sha1 KexAlgorithms that work with older versions of PHPStorm
RUN echo 'root:root' | chpasswd
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo KexAlgorithms diffie-hellman-group1-sha1 >> /etc/ssh/sshd_config

# Some tools we might need from time to time to mess around.
RUN apt-get install -y vim jq

EXPOSE 22
EXPOSE 80

CMD service ssh start && service apache2 start && sleep infinity
