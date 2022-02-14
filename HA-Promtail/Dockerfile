ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base/amd64:5.2.3
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64

RUN \
# Promtail Version
    promtail="v2.4.2" \
    \
    && ARCH="${BUILD_ARCH}" \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    && if [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="armhf"; fi \

    && apt-get update \
    && apt-get install -y --no-install-recommends \
        unzip \
# Get Promtail Bin fiel
    && curl -J -L -o /promtail.zip \
        "https://github.com/grafana/loki/releases/download/v2.4.2/promtail-linux-amd64.zip" \
# extract the binary
    && unzip /promtail.zip \
# make sure it is executable
    && chmod a+x /promtail-linux-amd64 \

# cleanup download
    && rm -fr \
        /promtail.zip

# Copy root filesystem
# escape=`
COPY promconfig.yml /etc/promtail/config.yml

ENTRYPOINT ["/promtail-linux-amd64"]

CMD ["-config.file=/etc/promtail/config.yml"]