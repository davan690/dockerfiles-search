FROM buildpack-deps:jessie-scm

# gcc for cgo
RUN apt-get update -y && apt-get install -y -q curl build-essential ca-certificates git mercurial
RUN apt-get install -y g++ gcc libc6-dev make pkg-config \
	&& rm -rf /var/lib/apt/lists/*
ENV GOLANG_VERSION 1.7.3
ENV GOLANG_DOWNLOAD_URL https://golang.org/dl/go$GOLANG_VERSION.linux-amd64.tar.gz
ENV GOLANG_DOWNLOAD_SHA256 508028aac0654e993564b6e2014bf2d4a9751e3b286661b0b0040046cf18028e

RUN curl -fsSL "$GOLANG_DOWNLOAD_URL" -o golang.tar.gz \
	&& echo "$GOLANG_DOWNLOAD_SHA256  golang.tar.gz" | sha256sum -c - \
	&& tar -C /usr/local -xzf golang.tar.gz \
	&& rm golang.tar.gz

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

COPY go-wrapper /usr/local/bin/

RUN cd ~ && git clone https://github.com/3846masa/ngrok
RUN cd ~/ngrok && make release-server && \
    mv ./bin/ngrokd /usr/local/bin/ngrokd
RUN chmod +x /usr/local/bin/ngrokd
ENTRYPOINT ["ngrokd"]
