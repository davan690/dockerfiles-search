FROM ubuntu:14.04

MAINTAINER Bram Plessers <webdevotion@gmail.com>

RUN apt-get update
RUN apt-get install -y nginx
RUN echo 'Hi, I am in your container!' > /usr/share/nginx/html/index.html

CMD ["nginx","-g","daemon off;"]
expose 80
