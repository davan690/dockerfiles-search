FROM ubuntu:18.04

RUN apt update && \
  apt install -y build-essential \
  unzip \
  git \
  ninja-build \
  curl \
  tar

RUN curl -L -O https://cmake.org/files/v3.12/cmake-3.12.1-Linux-x86_64.tar.gz && \
  curl https://cmake.org/files/v3.12/cmake-3.12.1-SHA-256.txt | grep Linux-x86_64.tar.gz | sha256sum -c &&\
  mkdir cmake && \
  tar -xf cmake-3.12.1-Linux-x86_64.tar.gz --directory=/usr/ --strip-components=1 && \
  rm -f cmake-3.12.1-Linux-x86_64.tar.gz

WORKDIR /opt/
RUN curl -L -O https://dl.bintray.com/conan/installers/conan-ubuntu-64_1_6_1.deb && \
  dpkg -i conan-ubuntu-64_1_6_1.deb && \
  rm -f conan-ubuntu-64_1_6_1.deb && \
  git clone https://github.com/Microsoft/vcpkg.git vcpkg && \
  ./vcpkg/bootstrap-vcpkg.sh && \
  update-alternatives --install /usr/bin/vcpkg vcpkg /opt/vcpkg/vcpkg 1

WORKDIR /