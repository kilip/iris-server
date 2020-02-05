FROM alpine:latest

LABEL maintainer="me@itstoni.com"

RUN apk add --no-cache \
    squid \
    dcron

# install sarg
RUN apk add --virtual build-dependencies \
        build-base \
        gcc \
        wget \
    && apk add \
        bash

WORKDIR /tmp
RUN wget -O sarg.tar.gz -c https://sourceforge.net/projects/sarg/files/sarg/sarg-2.4.0/sarg-2.4.0.tar.gz/download \
    && tar -xzvf sarg.tar.gz \
    && cd sarg-2.4.0 \
    && ./configure \
    && make \
    && make install

COPY start.sh /usr/local/bin/
COPY docker-cmd.sh /usr/local/bin/
COPY squid.conf /etc/squid/
COPY sarg.conf /usr/local/etc/
RUN chmod +x /usr/local/bin/start.sh

VOLUME [ "/squid-reports" ]
RUN  rm -rf /var/cache/apk/*

CMD ["/usr/local/bin/docker-cmd.sh"]
ENTRYPOINT ["/usr/local/bin/start.sh"]