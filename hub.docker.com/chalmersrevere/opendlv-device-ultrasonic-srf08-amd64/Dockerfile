# Copyright (C) 2018 Ola Benderius
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

FROM alpine:3.7 as builder
RUN apk update && \
  apk --no-cache add \
    cmake \
    g++ \
    make \
    upx \
    ncurses-dev \
    linux-headers

RUN apk add libcluon --no-cache --repository https://chrberger.github.io/libcluon/alpine/v3.7 --allow-untrusted

ADD . /opt/sources
WORKDIR /opt/sources
RUN rm -rf build && \
    mkdir build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/tmp/build-dest .. && \
    make && make test && make install && upx -9 /tmp/build-dest/bin/opendlv-device-ultrasonic-srf08


FROM alpine:3.7

RUN apk update && \
    apk --no-cache add \
    ncurses-dev 

WORKDIR /usr/bin
COPY --from=builder /tmp/build-dest/ /usr/
CMD ["/usr/bin/opendlv-device-ultrasonic-srf08"]
