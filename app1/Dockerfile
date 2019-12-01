FROM php:7.2-apache

RUN apt-get update && \
    apt-get install -y --no-install-recommends 
RUN docker-php-ext-install pdo pdo_mysql mysqli

# Configure Apache & clean
RUN a2enmod rewrite  
# ======= composer =======
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get install -y git zip unzip

# Authorize SSH Host
RUN mkdir -p /root/.ssh && \
    chmod 0700 /root/.ssh && \
    ssh-keyscan gitlab.com > /root/.ssh/known_hosts

#SSH add
ADD id_rsa /root/.ssh
ADD id_rsa.pub /root/.ssh
RUN chmod 600 /root/.ssh/id_rsa && \
    chmod 600 /root/.ssh/id_rsa.pub

#Clone project
RUN git clone git@gitlab.com:example/example.git app

WORKDIR /var/www/html/app

RUN composer install

ADD .env ./
RUN chmod -R 777 storage

#Apache setup
ADD 000-default.conf /etc/apache2/sites-available
RUN service apache2 restart

EXPOSE 80

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]