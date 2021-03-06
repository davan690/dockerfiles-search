FROM golang:1 AS build

ARG VERSION="0.20.0"
ARG CHECKSUM="4789ef95b09ba86a66a2923c3535d1bfe30a566390770784c52052c7c83ee1bc"

ADD https://github.com/prometheus/alertmanager/archive/v$VERSION.tar.gz /tmp/alertmanager.tar.gz

RUN [ "$CHECKSUM" = "$(sha256sum /tmp/alertmanager.tar.gz | awk '{print $1}')" ] && \
    tar -C /tmp -xf /tmp/alertmanager.tar.gz && \
    mkdir -p /go/src/github.com/prometheus && \
    mv /tmp/alertmanager-$VERSION /go/src/github.com/prometheus/alertmanager

RUN apt update && \
    apt install -y ca-certificates && \
    cd /go/src/github.com/prometheus/alertmanager && \
      make build && \
    echo "nogroup:*:100:nobody" > /tmp/group && \
    echo "nobody:*:100:100:::" > /tmp/passwd && \
    mkdir -p /tmp/data


FROM scratch

COPY --from=build --chown=100:100 /go/src/github.com/prometheus/alertmanager/alertmanager /
COPY --from=build --chown=100:100 /tmp/data /data
COPY --from=build --chown=100:100 /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=build --chown=100:100 /tmp/group \
                                  /tmp/passwd \
                                  /etc/

USER 100:100
VOLUME ["/data"]
EXPOSE 9093/tcp
ENTRYPOINT ["/alertmanager"]
