FROM golang:alpine AS builder

ARG VERSION="v0.10.0"

WORKDIR /

RUN set -ex \ 
	&& apk add --no-cache make git && mkdir /release  \
	&& git clone --branch=$VERSION  https://github.com/p4gefau1t/trojan-go.git  \
	&& cd trojan-go && make  && cp build/trojan-go /release/trojan-go && rm trojan-go -rf \
       	&& wget https://github.com/v2ray/domain-list-community/raw/release/dlc.dat -O /release/geosite.dat \
	&& wget https://github.com/v2ray/geoip/raw/release/geoip.dat -O /release/geoip.dat

FROM alpine:latest

COPY --from=builder /release /usr/bin/trojan-go/

RUN set -ex && apk add --no-cache tzdata libcap \
            && setcap 'cap_net_bind_service=+ep' /usr/bin/trojan-go/trojan-go
			
ENV TZ=Asia/Shanghai

WORKDIR /config

CMD ["/bin/sh", "/wait_for_caddy.sh", "/usr/bin/trojan-go/trojan-go", "-config", "config.json"]
