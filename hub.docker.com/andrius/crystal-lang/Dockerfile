FROM crystallang/crystal:0.31.0-build

LABEL maintainer="Andrius Kairiukstis <k@andrius.mobi>"

RUN \
apt-get update && \
DEBIAN_FRONTEND=noninteractive \
apt-get install -yqq --no-install-recommends --no-install-suggests \
  curl \
  dnsutils \
  iputils-ping \
  jq \
  libc-dev \
  libpq-dev \
  libsqlite3-dev \
  netcat-traditional \
  ngrep \
  nmap \
  mc \
  tcpdump \
  tmux \
  vim-nox \
&& : \
cd /tmp && \
git clone https://github.com/crystal-community/icr.git && \
cd icr && \
make && \
make install && \
cd .. && \
rm -rf icr && \
icr --disable-update-check && \
\
apt-get clean all && \
rm -rf /tmp/* \
       /var/tmp/* \
       /var/lib/apt/lists/*

CMD ["icr", "-r", "colorize", "-r"]
