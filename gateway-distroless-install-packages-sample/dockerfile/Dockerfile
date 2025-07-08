# Copyright (c) 2025 Broadcom Inc. and its subsidiaries. All Rights Reserved.


ARG BUILDER_IMAGE=docker.io/debian:12

ARG BASEDIR=/tmp/base-root


# Provide Packages
###################
FROM ${BUILDER_IMAGE} AS builder

USER root
COPY packages .
RUN PACKAGES="$(cat packages)" && \
    cd /tmp && \
    apt-get update && \ 
    apt-get install -y apt-rdepends && \
    # Download curl ,jq and all dependencies in its dependency tree as mentioned at https://packages.debian.org/bookworm/curl 
    apt-get download curl \
                    libbrotli1 \
                    libc6 \
                    libgssapi-krb5-2 \
                    libidn2-0 \
                    libssh2-1 \
                    libldap-2.5-0 \
                    libnghttp2-14 \
                    libpsl5 \
                    librtmp1 \
                    libssh-4 \
                    libssl3 \
                    libzstd1 \
                    libhogweed6 \
                    libnettle8 \
                    libffi8 \
                    libsasl2-2 \
                    libp11-kit0 \
                    libtasn1-6 \
                    libgnutls30 \
                    zlib1g \
                    libunistring2 \
                     libcurl4 && \
        apt-get download jq \
                        libjq1 \
                        libc6 \
                        libonig5 \
                        libgcc-s1 \
                        libjq1 && \
    mkdir /dpkg && \
    mkdir -p /dpkg/var/lib/dpkg/status.d && \
    for deb in *.deb; do \
        package_name=$(dpkg-deb -I ${deb} | awk '/^ Package: .*$/ {print $2}'); \
        echo "Processing: ${package_name}"; \
        dpkg --ctrl-tarfile $deb | tar -Oxf - ./control > /dpkg/var/lib/dpkg/status.d/${package_name}; \
        dpkg --extract $deb /dpkg || exit 10; \
    done

FROM caapim/gateway:latest

USER root
COPY --from=builder /dpkg /

USER 1001
WORKDIR /
