FROM debian:9.0

MAINTAINER Eduardo Pinuaga <info@did-web.com>

RUN apt-get update && \
    apt-get install -y apache2 && \
    apt-get install -y vim && \
    apt-get clean && rm -rf /var/lib/apt/lists/*


ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

EXPOSE 80

ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
