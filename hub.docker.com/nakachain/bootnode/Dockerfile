# Build in a stock Go builder container
FROM golang:1.11-alpine as builder

RUN apk add --no-cache make gcc musl-dev linux-headers

ADD . /go-naka

ARG GIT_BRANCH=master
RUN cd /go-naka \
    && echo "Branch: ${GIT_BRANCH}" \
    && make all git-branch=${GIT_BRANCH}

# Pull Geth into a second stage deploy alpine container
FROM alpine:latest

RUN apk add --no-cache ca-certificates
COPY --from=builder /go-naka/build/bin/bootnode /usr/local/bin/

EXPOSE 30301 30301/udp
ENTRYPOINT ["bootnode"]
