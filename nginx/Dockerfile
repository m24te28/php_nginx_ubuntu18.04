FROM ubuntu:18.04

LABEL maintainer="m24te28"

RUN apt-get update && \
    apt-get install -y -q curl gnupg2
RUN curl http://nginx.org/keys/nginx_signing.key | apt-key add -

RUN apt-get update && \
    apt-get install -y -q nginx

ADD nginx.conf /etc/nginx/
ADD server.conf /etc/nginx/conf.d

EXPOSE 443 80

CMD ["nginx", "-g", "daemon off;"]