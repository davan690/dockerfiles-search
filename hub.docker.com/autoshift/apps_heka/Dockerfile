FROM alpine
ENV HEKA_TAG="v0.10.0"

RUN apk -U add bzr mercurial git cmake go alpine-sdk bash perl && \
    git clone https://github.com/mozilla-services/heka.git && \
    cd /heka && \
    git checkout $HEKA_TAG && \
    bash -c "source ./build.sh && make package" && \
    tar xvzf /heka/build/heka-*.tar.gz -C /usr --strip-components=1 && \
    rm -rf /heka && \
    apk del bzr mercurial git cmake go alpine-sdk bash perl && \
    rm -rf /var/cache/apk/*

COPY ./heka-config.toml /heka-config.toml

CMD ["/usr/bin/hekad","--config","/heka-config.toml"]
