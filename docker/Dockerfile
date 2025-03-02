ARG DEBIAN_VERSION=12

FROM debian:${DEBIAN_VERSION} AS builder

ARG NGINX_VERSION=1.26.3

WORKDIR /usr/local/src/

RUN set -xe \
    && apt-get update -y \
    && apt-get --no-install-recommends --no-install-suggests -y install \
        build-essential ca-certificates git wget lsb-release \
        libpcre2-dev libpcre3-dev zlib1g-dev libssl-dev libxml2-utils \
        xsltproc mercurial devscripts fakeroot debhelper dpkg-dev quilt \
        libparse-recdescent-perl libbrotli-dev \
    && rm -rf /var/lib/apt/lists/*

RUN set -xe \
    && wget https://github.com/nginx/pkg-oss/archive/refs/tags/${NGINX_VERSION}-1.tar.gz -O pkg-oss.tar.gz \
    && tar -xzf pkg-oss.tar.gz \
    && mv pkg-oss-${NGINX_VERSION}-1 pkg-oss \
    && rm pkg-oss.tar.gz

RUN set -xe \
    && cd /usr/local/src/pkg-oss/debian && make module-brotli

FROM scratch

COPY --from=builder /usr/local/src/pkg-oss/debian/debuild-module-brotli /
