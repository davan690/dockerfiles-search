FROM mongo

# This is needed for boot2docker. See: https://github.com/boot2docker/boot2docker/issues/581
RUN usermod -u 1000 mongodb
RUN usermod -G staff mongodb

RUN chown -R mongodb:staff /data/db
RUN chmod -R 770 /data/db
