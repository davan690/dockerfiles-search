
FROM ubuntu:14.04

MAINTAINER Thomas Boquet <thomas.boquet@hec.ca>

ENV DEBIAN_FRONTEND noninteractive
RUN locale-gen en_US.UTF-8 && /usr/sbin/update-locale LANG=en_US.UTF-8
ENV LANG en_US.UTF-8

# proper init to handle signal propagation and zombie reaping
ADD https://github.com/krallin/tini/releases/download/v0.8.4/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]

RUN apt-get update && \
    apt-get -y install \
        wget \
        libssl-dev \
        gfortran \
        build-essential \
        make \
        gcc \
        build-essential \
        git-core \
        vim-tiny \
        nano \
        libffi-dev \
        python-pip \
        python-software-properties \
        software-properties-common && \
    add-apt-repository -y ppa:fkrull/deadsnakes && \
    apt-get update && \
    apt-get -y install \
        python2.6 \
        python2.7 \
        python3.3 \
        python3.4 \
        python3.5 \
        pypy \
        python2.6-dev \
        python2.7-dev \
        python3.3-dev \
        python3.4-dev \
        python3.5-dev \
        pypy-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir /install && \
    wget -O /install/pypy3-2.4-linux_x86_64-portable.tar.bz2 \
            "https://bitbucket.org/squeaky/portable-pypy/downloads/pypy3-2.4-linux_x86_64-portable.tar.bz2" && \
    tar jxf /install/pypy3-*.tar.bz2 -C /install && \
    rm /install/pypy3-*.tar.bz2 && \
    ln -s /install/pypy3-*/bin/pypy3 /usr/local/bin/pypy3

RUN pip install -U pip && pip install tox


RUN apt-get update --fix-missing && apt-get install -y libopenblas-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# RUN mkdir -p /root/src && \
#     cd /root/src/ && \
#     wget http://www.netlib.org/lapack/lapack.tgz && \
#     tar xzf lapack.tgz && \
#     cd lapack-*/ && \
#     cp INSTALL/make.inc.gfortran make.inc && \
#     sed -i 's/OPTS     = -O2/OPTS = -O2 -fPIC -m64/g' make.inc && \
#     sed -i 's/NOOPT    = -O0/NOOPT = -O0 -fPIC -m64/g' make.inc && \
#     make lapacklib && \
#     make clean && \
#     cp liblapack.a /usr/local/lib && \
#     echo "Created liblapack.a in /usr/local/lib" && \
#     rm -r -f /root/src && \

# RUN apt-get install libblas-dev liblapack-dev
WORKDIR /app
VOLUME /src

ONBUILD COPY install-prereqs*.sh requirements*.txt tox.ini /app/
ONBUILD ARG SKIP_TOX=false
ONBUILD RUN bash -c " \
    if [ -f '/app/install-prereqs.sh' ]; then \
        bash /app/install-prereqs.sh; \
    fi && \
    if [ $SKIP_TOX == false ]; then \
        TOXBUILD=true tox; \
    fi"

CMD cp -rT /src/ /app/ && tox
