FROM ubuntu:18.04

MAINTAINER Ghislain MARY <ghislain.mary@belledonne-communications.com>


# Configure locale
RUN apt-get update && \
    apt-get install -y locales && \
    apt-get clean && \
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

ENV SHELL=/bin/bash
#ENV PS1='\[\e[33m\]\u@bc-dev-ubuntu-18.04>\[\e[0m\] '

# Install common general tools
RUN apt-get update && \
    apt-get install -y nano sudo vim && \
    apt-get clean

# Install development tools
RUN apt-get update && \
    apt-get install -y alien at autoconf bison ccache clang cmake doxygen elfutils g++ gdb git graphviz intltool libtool lsb-release make nasm ninja-build openssh-client patch python-pip python-pystache python-six yasm && \
    apt-get clean && \
    pip install sphinx javasphinx sphinx_csharp

# Install linphone & flexisip dependencies development packages
RUN apt-get update && \
    apt-get install -y libasound2-dev libavcodec-dev libavutil-dev libbsd-dev libegl1-mesa-dev libglew-dev libgsm1-dev libjansson-dev libmariadb-dev-compat libmbedtls-dev libopus-dev libpq-dev libprotobuf-dev libpulse-dev libqt5svg5-dev libsnmp-dev libspeex-dev libspeexdsp-dev libsqlite3-dev libsrtp2-dev libssl-dev libswscale-dev libturbojpeg0-dev libv4l-dev libvpx-dev libxerces-c-dev libxml2-dev libxv-dev protobuf-compiler qt3d5-dev qt5-default qtbase5-dev qtbase5-dev-tools qtconnectivity5-dev qtdeclarative5-dev qtdeclarative5-dev-tools qtdeclarative5-dev-tools qtquickcontrols2-5-dev qttools5-dev qttools5-dev-tools xsdcxx && \
    apt-get clean

# Configure user bc
RUN useradd -ms /bin/bash bc && \
    echo 'bc:cotcot' | chpasswd && \
    echo 'bc ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER bc
WORKDIR /home/bc
COPY --chown=bc rpmmacros /home/bc/.rpmmacros
CMD bash
