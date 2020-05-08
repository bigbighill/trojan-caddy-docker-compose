FROM golang:alpine AS builder

ARG VERSION='v0.4.7'

WORKDIR /

RUN set -ex \ 
	&& apk add --no-cache --update git  && mkdir release \
	&& git clone --depth=1 https://github.com/p4gefau1t/trojan-go.git  \
	&& cd trojan-go && git checkout $VERSION -b $VERSION  && go build -tags "full" -ldflags "-s -w" -o /release \
	&& wget https://github.com/v2ray/domain-list-community/raw/release/dlc.dat -O release/geosite.dat \
	&& wget https://github.com/v2ray/geoip/raw/release/geoip.dat -O release/geoip.dat

FROM alpine:latest

COPY --from=builder /release /usr/bin/trojan-go/

RUN set -ex \
	&& apk -add --no-cache \
			tzdata
ENV TZ=Asia/Shanghai

WORKDIR /config

CMD ["/bin/sh", "/wait_for_caddy.sh", "/usr/bin/trojan-go/trojan-go", "-config", "config.json"]
