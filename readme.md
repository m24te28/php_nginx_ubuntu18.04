# about
create nginx and php docker container on ubuntu:18.04-based docker image.
https://www.m24te28.com/entry/20190217/1550329200
## tl;dr

clone repository and exec
```sh
docker-compose up -d
```
then you can access phpinfo at your localhost.

## env
* ubuntu:18.04
* nginx:1.14.0
* php:5.6

### communication nginx and php containers
In default, nginx and php communicate via http.

If you wanna via socket, you need to chenge some files and prepare a volume to share a socket file.

#### files to change
https://github.com/m24te28/php_nginx_ubuntu18.04/commit/0561e3404d4e124623100f8e03724effd45bea2e
* docker-compose.yml
```yaml
# - socket:/var/run/php
- ./html:/var/www/html
↓
- socket:/var/run/php
- ./html:/var/www/html

...

# volumes:
#   socket:
#     external: true
↓
volumes:
  socket:
    external: true
```
* nginx/server.conf
```yaml
# fastcgi_pass unix:/var/run/php/php5.6-fpm.sock;
fastcgi_pass php:9000;
↓
fastcgi_pass unix:/var/run/php/php5.6-fpm.sock;
# fastcgi_pass php:9000;
```
* php/Dockerfile
```yaml
# sed -i -e "s/\/run\/php\/php5.6-fpm.sock/\/var\/run\/php\/php5.6-fpm.sock/g" /etc/php/5.6/fpm/pool.d/www.conf && \
sed -i -e "s/\/run\/php\/php5.6-fpm.sock/9000/g" /etc/php/5.6/fpm/pool.d/www.conf && \
↓
sed -i -e "s/\/run\/php\/php5.6-fpm.sock/\/var\/run\/php\/php5.6-fpm.sock/g" /etc/php/5.6/fpm/pool.d/www.conf && \
# sed -i -e "s/\/run\/php\/php5.6-fpm.sock/9000/g" /etc/php/5.6/fpm/pool.d/www.conf && \
```
#### create external volume
```sh
docker volume create --name=socket
```
