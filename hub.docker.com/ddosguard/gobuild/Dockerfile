FROM golang:1.12-alpine

RUN set -x \
  && apk add -q --update --clean-protected --no-cache \
    git \
    && rm -rf /var/cache/apk/*\
    && mkdir /.cache\
    && chmod 777 /.cache\
    && go get github.com/libp2p/go-reuseport\
    && go get github.com/ugorji/go/codec\
    && go get github.com/vmihailenco/msgpack\
    && go get github.com/nats-io/go-nats-streaming\
    && go get github.com/nats-io/go-nats\
    && go get gopkg.in/yaml.v2\
    && go get github.com/fsnotify/fsnotify\
    && go get github.com/nats-io/go-nats\
    && go get github.com/vmihailenco/msgpack\
    && go get github.com/kshvakov/clickhouse\
    && go get github.com/oschwald/maxminddb-golang\
    && go get github.com/lib/pq\
    && go get github.com/patrickmn/go-cache\
    && go get github.com/spf13/viper\
    && go get github.com/go-acme/lego/cmd/lego\
    && go get go.etcd.io/etcd/clientv3
