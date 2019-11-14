FROM amd64/ubuntu:rolling

# Dependencies needed to compile openblack
RUN apt-get update && apt-get upgrade -y && apt-get dist-upgrade -y && \
    apt-get install -y --no-install-recommends \
    \
    build-essential \
    bzip2 \
    ca-certificates \
    curl \
    git \
    libbullet-dev \
    libsdl2-dev \
    ninja-build

# Install cmake 3.15.4
RUN curl -s "https://cmake.org/files/v3.15/cmake-3.15.4-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C /usr/local

# Install vcpkg (https://github.com/Microsoft/vcpkg.git) to compile the dependencies that do not have Ubuntu packages.
# Some packages are avaliable in distro package managers, but we use vcpkg specifically for:
# - spdlog: we need at least 1.3.0
# - spirv-tools: newer versions export CMake targets that we use
# - glm: small header only, better to be on latest
# Packages not avaliable in distro package manager:
# - entt
# - spirv-cross
RUN mkdir /tools \
    && cd /tools \
    && git clone https://github.com/Microsoft/vcpkg.git --depth 1 \ 
    && cd vcpkg \
    && ./bootstrap-vcpkg.sh --useSystemBinaries \
    && VCPKG_FORCE_SYSTEM_BINARIES=1 ./vcpkg install cxxopts entt glm spdlog spirv-cross spirv-tools

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app