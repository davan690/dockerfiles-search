FROM playn/alpine:edge

#Base settings
ENV HOME /root

#Install zeronet
RUN apk --update --no-cache --no-progress add musl-dev gcc python python-dev py2-pip tor && \
    pip install gevent msgpack-python && \
    apk del musl-dev gcc python-dev py2-pip && \
    rm -rf /var/cache/apk/* /tmp/* /var/tmp/* && \
    echo "ControlPort 9051" >> /etc/tor/torrc && \
    echo "CookieAuthentication 1" >> /etc/tor/torrc && \
    rm -rf /var/cache/apk/*

#Add Zeronet source
#COPY . /root
#VOLUME /root/data

#Control if Tor proxy is started
ENV ENABLE_TOR false

WORKDIR /root/zeronet

#Set upstart command
CMD (! ${ENABLE_TOR} || tor&) && python zeronet.py --ui_ip 0.0.0.0

#Expose ports
EXPOSE 43110 15441