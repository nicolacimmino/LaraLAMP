FROM php:7.1-apache

RUN apt-get update && apt-get -y install git curl php5-mcrypt php5-json zip vim jq
RUN /usr/sbin/a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# Prepare /var/www so that files uploaded by phpstorm can be used
# by www-data. We set the sticky bit on /var/www and group and owner
# rwx. The sticky bit causes all files uploaded to inherit the permissions.
RUN chown www-data:www-data -R /var/www/
RUN rm /var/www/html -rf
RUN mkdir -p /var/www/laravel/storage 
RUN mkdir -p /var/www/laravel/bootstrap/cache
RUN mkdir -p /var/www/public
RUN chmod 1771 /var/www -R

# Change default website root to /var/www/public
RUN rm /etc/apache2/sites-enabled/000-default.conf
COPY laravel-site.conf /etc/apache2/sites-enabled/

# Install ssh so we can use sftp from PHPStorm and avoid volumes
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

# Create a user belonging to www-data group so it can upload the site pages.
# Also allow older sha1 KexAlgorithms that work with older versions of PHPStorm
RUN echo KexAlgorithms diffie-hellman-group1-sha1 >> /etc/ssh/sshd_config
RUN useradd phpstorm -g www-data -s /bin/bash -m
RUN echo phpstorm:phpstorm | chpasswd

EXPOSE 22
EXPOSE 80

CMD service ssh start && service apache2 start && sleep infinity
