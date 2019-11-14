ARG DISTRO=debian
ARG RELEASE=stretch-slim
FROM ${DISTRO}:${RELEASE}

ENV DEBIAN_FRONTEND=noninteractive

LABEL \
    maintainer="info@openblack.dev" \
    org.label-schema.description="openblack base images" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://openblack.dev/" \
    org.label-schema.vcs-url="https://github.com/openblack/compile-farm" \
    org.label-schema.vendor=openblack

# Dependencies needed to compile openblack
RUN apt-get update && apt-get upgrade -y && apt-get dist-upgrade -y && \
    apt-get install -y --no-install-recommends \
    \
    bzip2 \
    ca-certificates \
    cmake \
    git \
    libc6-dev \
    libfile-fcntllock-perl \
    libfontconfig-dev \
    libicu-dev \
    liblzma-dev \
    liblzo2-dev \
    libsdl2-dev \
    make \
    software-properties-common \
    tar \
    xz-utils \
    && rm -rf /var/lib/apt/lists/*
