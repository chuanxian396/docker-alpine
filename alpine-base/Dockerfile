FROM alpine:3.12

# Add s6-overlay
ENV S6_OVERLAY_VERSION=v2.2.0.3 \
    GO_DNSMASQ_VERSION=1.0.7

RUN mv /etc/apk/repositories /etc/apk/repositories.bak && \
    echo "https://mirrors.aliyun.com/alpine/v3.12/main" >/etc/apk/repositories && \
    echo "https://mirrors.aliyun.com/alpine/v3.12/community" >>/etc/apk/repositories && \
    apk add --update --no-cache bind-tools curl libcap && \
    curl -sSL https://dl.ghpig.top/https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz \
    | tar xfz - -C / && \
    curl -sSL https://dl.ghpig.top/https://github.com/janeczku/go-dnsmasq/releases/download/${GO_DNSMASQ_VERSION}/go-dnsmasq-min_linux-amd64 -o /bin/go-dnsmasq && \
    chmod +x /bin/go-dnsmasq && \
    apk del curl && \
    # create user and give binary permissions to bind to lower port
    addgroup go-dnsmasq && \
    adduser -D -g "" -s /bin/sh -G go-dnsmasq go-dnsmasq && \
    setcap CAP_NET_BIND_SERVICE=+eip /bin/go-dnsmasq
    
COPY root /

ENTRYPOINT ["/init"]
CMD []
