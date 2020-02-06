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
    libbullet-extras-dev \
    libsdl2-dev \
    ninja-build

# Install cmake 3.15.4
RUN curl -s "https://cmake.org/files/v3.15/cmake-3.15.4-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C /usr/local

# Install clang-format
RUN curl -s 'https://github.com/angular/clang-format/blob/master/bin/linux_x64/clang-format?raw=true' -o '/usr/local/bin/clang-format' \
    && chmod +x /usr/local/bin/clang-format

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
    && VCPKG_FORCE_SYSTEM_BINARIES=1 ./vcpkg install cxxopts entt glm spdlog

# build bgfx (release only for now..)
WORKDIR /bgfx
RUN git -c http.sslVerify=false clone --recursive -b master https://github.com/openblack/bgfx.cmake . \
 && mkdir build \
 && cd build \
 && cmake .. -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/bgfx-install \
 && ninja install \
 && rm -rf /bgfx

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app