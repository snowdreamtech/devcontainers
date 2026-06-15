# DevContainers Image (Debian Variant)
# Extends snowdreamtech/debian with additional tooling and configuration.

FROM snowdreamtech/debian:13.5.0

# Build-time Configuration
ARG BUILDTIME \
    VERSION \
    REVISION \
    DEBIAN_FRONTEND=noninteractive \
    KEEPALIVE=0 \
    CAP_NET_BIND_SERVICE=0 \
    LANG=C.UTF-8 \
    UMASK=022 \
    DEBUG=false \
    PASSWORDLESS_SUDO=false \
    PGID=0 \
    PUID=0 \
    USER=root \
    WORKDIR=/root \
    UNIRTM_VERSION=0.24.0 \
    TEMPLATE_TAG=v0.9.2

# Standard OCI Metadata (https://github.com/opencontainers/image-spec)
LABEL org.opencontainers.image.authors="Snowdream Tech" \
    org.opencontainers.image.title="DevContainers Image Based On Debian" \
    org.opencontainers.image.description="Docker Images for DevContainers on Debian. (i386, amd64, arm32v5, arm32v7, arm64, mips64le, ppc64le, s390x)" \
    org.opencontainers.image.documentation="https://hub.docker.com/r/snowdreamtech/devcontainers" \
    org.opencontainers.image.base.name="snowdreamtech/devcontainers:debian" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.source="https://github.com/snowdreamtech/devcontainers" \
    org.opencontainers.image.vendor="Snowdream Tech" \
    org.opencontainers.image.version="${VERSION:-13.5.0}" \
    org.opencontainers.image.created="${BUILDTIME}" \
    org.opencontainers.image.revision="${REVISION}" \
    org.opencontainers.image.url="https://github.com/snowdreamtech/devcontainers"

# Runtime Environment Configuration
ENV DEBIAN_FRONTEND=${DEBIAN_FRONTEND} \
    KEEPALIVE=${KEEPALIVE} \
    CAP_NET_BIND_SERVICE=${CAP_NET_BIND_SERVICE} \
    LANG=${LANG} \
    UMASK=${UMASK} \
    DEBUG=${DEBUG} \
    PASSWORDLESS_SUDO=${PASSWORDLESS_SUDO} \
    PGID=${PGID} \
    PUID=${PUID} \
    USER=${USER} \
    WORKDIR=${WORKDIR}

# Additional tooling and configuration for DevContainers image
RUN set -eux \
    && DEBIAN_FRONTEND=noninteractive apt-get -qqy update \
    && DEBIAN_FRONTEND=noninteractive apt-get -qqy install --no-install-recommends \
    bash \
    vim \
    make \
    git \
    curl \
    ca-certificates \
    gpg \
    libstdc++6 \
    libatomic1 \
    && DEBIAN_FRONTEND=noninteractive apt-get -qqy --purge autoremove \
    && DEBIAN_FRONTEND=noninteractive apt-get -qqy clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* \
    && rm -rf /var/tmp/*

# Configure unirtm (polyglot runtime manager) environment variables
ENV UNIRTM_VERSION=${UNIRTM_VERSION} \
    UNIRTM_DATA_DIR="/unirtm" \
    UNIRTM_CONFIG_DIR="/unirtm" \
    UNIRTM_CACHE_DIR="/unirtm/cache" \
    PATH="/unirtm/shims:$PATH"

# Install unirtm and setup runtime environments
RUN curl -fsSL https://raw.githubusercontent.com/snowdreamtech/UniRTM/main/install.sh | UNIRTM_VERSION=${UNIRTM_VERSION} INSTALL_DIR=/usr/local/bin sh \
    && unirtm trust -a && unirtm install

# Configure development tools from template
RUN git clone --depth 1 --branch ${TEMPLATE_TAG} https://github.com/snowdreamtech/template.git /tmp/template \
    && cd /tmp/template \
    && make setup \
    && cp .unirtm.toml /unirtm/unirtm.toml \
    && cd / \
    && rm -rf /tmp/template \
    && cd /unirtm \
    && unirtm trust -a \
    && (unirtm install || true)

# Container Orchestration Files
COPY entrypoint.d /usr/local/bin/entrypoint.d

RUN chmod +x /usr/local/bin/entrypoint.d/*

# Standard Healthcheck (Verification of core system responsiveness)
HEALTHCHECK NONE

# Process Execution
USER root
WORKDIR /root
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["/bin/bash"]
