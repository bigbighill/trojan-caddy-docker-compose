FROM alpine:latest

ARG VERSION='v0.1.3'


RUN mkdir /tmp/repo \ 
 && apk add --no-cache \
      unzip \
      tzdata \
 && cd /tmp/repo \
 && wget --no-check-certificate https://github.com/p4gefau1t/trojan-go/releases/download/$VERSION/trojan-go-linux-amd64.zip \
 && unzip -d/tmp/repo/trojan-go /tmp/repo/trojan-go-linux-amd64.zip  \
 && mv /tmp/repo/torjan-go/ /usr/bin \
 && rm -rf /tmp/repo

ENV TZ=Asia/Shanghai

WORKDIR /config

CMD ["/bin/sh", "/wait_for_caddy.sh", "/usr/bin/trogjan-go/trojan", "config.json"]
