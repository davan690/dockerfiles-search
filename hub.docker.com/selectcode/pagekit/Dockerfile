FROM ubuntu:trusty
EXPOSE 80

RUN apt-get update && \
    apt-get -y install \
    nginx \
    unzip \
    wget \
    ca-certificates \
    php5 php5-fpm php5-cli php5-json php5-mysql php5-curl

RUN mkdir /pagekit
WORKDIR /pagekit
VOLUME ["/pagekit/storage", "/pagekit/app/cache"]

RUN wget https://github.com/pagekit/pagekit/releases/download/1.0.13/pagekit-1.0.13.zip -O /pagekit/pagekit.zip && \
    unzip /pagekit/pagekit.zip && rm /pagekit/pagekit.zip

ADD nginx.conf /etc/nginx/nginx.conf

RUN chown -R www-data: /pagekit && \
    apt-get autoremove wget unzip -y && \
    apt-get autoclean -y && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["sh", "-c", "service php5-fpm start && nginx"]
