FROM mysql:5.7

COPY docker-entrypoint.sh replication-entrypoint.sh init-slave.sh /usr/local/bin/

RUN ln -s usr/local/bin/replication-entrypoint.sh /replication-entrypoint.sh \
    && ln -s usr/local/bin/init-slave.sh /init-slave.sh

ENV MYSQL_REPLICATION_USER replication
ENV MYSQL_REPLICATION_PASSWORD replication_pass
ENV MYSQL_MASTER_PORT 3306
ENV MYSQL_DATABASE_DEFAULT_CHARSET utf8mb4
ENV MYSQL_DATABASE_DEFAULT_COLLATE utf8mb4_unicode_ci
ENV EXPIRE_LOGS_DAYS 60
ENV MAX_BINLOG_SIZE 100M

ENTRYPOINT ["/replication-entrypoint.sh"]

CMD ["mysqld"]
