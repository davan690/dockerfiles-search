#Installing all required packages
FROM centos:centos6
RUN yum install epel-release -y
RUN yum install cyrus-sasl-devel -y
RUN yum install mysql-devel -y
RUN yum install postgresql-libs -y
RUN yum install postgresql-devel -y
RUN yum install ImageMagick-devel -y
RUN yum install which -y
RUN yum install tar -y
RUN yum install wget -y
RUN yum install vim -y
RUN yum install nodejs -y

#Installing ruby 2.1.2 with RVM and passenger with nginx module
RUN curl -sSL https://rvm.io/mpapis.asc | gpg2 --import - && \
curl -L get.rvm.io | bash -s stable && \
source /etc/profile.d/rvm.sh && \
/bin/bash -l -c "rvm install 2.1.2" && \
/bin/bash -l -c "gem install rack -v=1.6.4" && \
/bin/bash -l -c "gem install bundle passenger --no-ri --no-rdoc" && \
yum install libcurl-devel -y && \
/bin/bash -l -c "passenger-install-nginx-module --auto --auto-download --prefix=/usr/local/nginx --extra-configure-flags='--with-http_gzip_static_module --with-http_ssl_module'"

#Adding nginx configuration and services files
ADD nginx.conf /usr/local/nginx/conf/nginx.conf
ADD nginx /etc/init.d/nginx
RUN /bin/bash -l -c "chmod +x /etc/init.d/nginx"

#Rails Application configuration
RUN mkdir -p /var/log/nginx/
ENV APP_PATH /app
WORKDIR $APP_PATH
ADD . $APP_PATH/
VOLUME [$APP_PATH]
EXPOSE 80 443

RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log
RUN echo "daemon off;" >> /usr/local/nginx/conf/nginx.conf
CMD ["/etc/init.d/nginx","start"]
