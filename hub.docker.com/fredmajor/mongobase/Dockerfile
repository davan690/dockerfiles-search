FROM debian:wheezy

# changed comparing to the standard image
RUN groupadd -g 1111 mongodb && useradd  -g mongodb -u 1111 mongodb

RUN apt-get update \
  && apt-get install -y curl \
  && rm -rf /var/lib/apt/lists/*

RUN curl -o /usr/local/bin/gosu -SL 'https://github.com/tianon/gosu/releases/download/1.1/gosu' \
  && chmod +x /usr/local/bin/gosu

RUN gpg --keyserver pgp.mit.edu --recv-keys DFFA3DCF326E302C4787673A01C4E7FAAAB2461C

ENV MONGO_VERSION 2.6.5

RUN curl -SL "https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-$MONGO_VERSION.tgz" -o mongo.tgz \
  && curl -SL "https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-$MONGO_VERSION.tgz.sig" -o mongo.tgz.sig \
  && gpg --verify mongo.tgz.sig \
  && tar -xvf mongo.tgz -C /usr/local --strip-components=1 \
  && rm mongo.tgz*

VOLUME /data/db

COPY docker-entrypoint.sh /entrypoint.sh
RUN chown mongodb:mongodb /entrypoint.sh
RUN chmod 744 /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

EXPOSE 27017
CMD ["mongod"]

