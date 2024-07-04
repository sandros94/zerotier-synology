# vim: ft=dockerfile

FROM alpine:latest as builder

WORKDIR /src
RUN apk add --no-cache rust cargo \
  && apk add  openssl-dev \
  && apk add --update alpine-sdk linux-headers \
  && git clone --quiet https://github.com/zerotier/ZeroTierOne.git /src \
  && git reset --quiet --hard ${ZTO_COMMIT} \
  && make -f make-linux.mk

FROM alpine:latest
LABEL version=${ZTO_VER}
LABEL description="ZeroTier One docker image for Synology NAS"

RUN apk add --update --no-cache bash jq libc6-compat libstdc++

EXPOSE 9993/udp
ENV ZT_ENABLE_MULTICORE=1 \
  ZT_CONCURRENCY=2 \
  ZT_CORE_PINNING=0 \
  MAX_WAIT_SECS=90 \
  SLEEP_TIME=15

COPY --from=builder /src/zerotier-one /usr/sbin/
RUN mkdir -p /var/lib/zerotier-one \
  && ln -s /usr/sbin/zerotier-one /usr/sbin/zerotier-idtool \
  && ln -s /usr/sbin/zerotier-one /usr/sbin/zerotier-cli

COPY entrypoint.sh /entrypoint.sh
RUN chmod 755 /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
