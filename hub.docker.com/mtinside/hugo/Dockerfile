FROM busybox:latest AS downloader

ARG HUGO_VERSION=0.45.1

RUN wget https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_${HUGO_VERSION}_Linux-64bit.tar.gz
RUN tar xzf hugo_${HUGO_VERSION}_Linux-64bit.tar.gz hugo

FROM scratch

COPY --from=downloader /hugo /hugo

WORKDIR /site
ONBUILD ADD . .
ONBUILD RUN ["/hugo"]
