# Copyright (C) 2019  Christian Berger
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

# Part to build cluon-recorder-remote-control.
FROM pipill/armhf-alpine:edge as builder
MAINTAINER Christian Berger "christian.berger@gu.se"

RUN [ "cross-build-start" ]

RUN echo http://dl-4.alpinelinux.org/alpine/v3.8/main > /etc/apk/repositories && \
    echo http://dl-4.alpinelinux.org/alpine/v3.8/community >> /etc/apk/repositories && \
    apk update && \
    apk --no-cache add \
        cmake \
        g++ \
        make
ADD . /opt/sources
WORKDIR /opt/sources
RUN mkdir build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/tmp .. && \
    make && make install

RUN [ "cross-build-end" ]


# Part to deploy cluon-recorder-remote-control.
FROM pipill/armhf-alpine:edge
MAINTAINER Christian Berger "christian.berger@gu.se"

WORKDIR /usr/bin
COPY --from=builder /tmp/bin/cluon-recorder-remote-control .
ENV NO_AT_BRIDGE=1
ENTRYPOINT ["/usr/bin/cluon-recorder-remote-control"]

