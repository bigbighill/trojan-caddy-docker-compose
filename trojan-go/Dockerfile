FROM alpine:latest

ARG VERSION='v0.4.1'


RUN set -ex \
        && mkdir /tmp/repo \ 
        && apk add --no-cache \
                    unzip \
                    tzdata \
        && cd /tmp/repo \
        && wget --no-check-certificate https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.31-r0/glibc-2.31-r0.apk \
        && apk add --repositories-file=/dev/null --allow-untrusted --no-network --no-cache /tmp/repo/glibc-2.31-r0.apk \
        && wget --no-check-certificate https://github.com/p4gefau1t/trojan-go/releases/download/$VERSION/trojan-go-linux-amd64.zip \
        && unzip /tmp/repo/trojan-go-linux-amd64.zip -d /tmp/repo/trojan-go \
        && cp /tmp/repo/trojan-go /usr/bin -r \
        && mv /usr/bin/trojan-go/trojan-go /usr/bin/trojan-go/trojan \
        && rm -rf /tmp/repo

ENV TZ=Asia/Shanghai
ENV PATH /usr/bin/trojan-go:$PATH

WORKDIR /config

CMD ["/bin/sh", "/wait_for_caddy.sh", "trojan", "-config", "config.json"]
