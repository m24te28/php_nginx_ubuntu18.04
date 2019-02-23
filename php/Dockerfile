FROM ubuntu:18.04

LABEL maintainer="m24te28"

RUN apt-get update && \
    apt-get install -y -q software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update && \
    apt-get install -y -q php5.6-fpm php5.6-mbstring php5.6-gd php5.6-mysql

RUN sed -i 's/;date.timezone\s*=/date.timezone\s*= UTC/g' /etc/php/5.6/fpm/php.ini && \
    sed -i "s/;error_log\s*=\s*php_errors.log/error_log = \/var\/log\/php_errors.log/" /etc/php/5.6/fpm/php.ini && \
    sed -i -e "s/\/run\/php\/php5.6-fpm.sock/\/var\/run\/php\/php5.6-fpm.sock/g" /etc/php/5.6/fpm/pool.d/www.conf && \
    # sed -i -e "s/\/run\/php\/php5.6-fpm.sock/9000/g" /etc/php/5.6/fpm/pool.d/www.conf && \
    sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/5.6/fpm/php-fpm.conf

# VOLUME ["/etc/php5","/var/run"]
RUN mkdir /run/php
EXPOSE 9000

ENTRYPOINT ["php-fpm5.6", "-F"]