# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.224.3/containers/cpp/.devcontainer/base.Dockerfile

# [Choice] Debian / Ubuntu version (use Debian 11, Ubuntu 18.04/21.04 on local arm64/Apple Silicon): debian-11, debian-10, ubuntu-21.04, ubuntu-20.04, ubuntu-18.04
ARG VARIANT="bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/cpp:0-${VARIANT}

# [Optional] Install CMake version different from what base image has already installed. 
# CMake reinstall choices: none, 3.21.5, 3.22.2, or versions from https://cmake.org/download/
ARG REINSTALL_CMAKE_VERSION_FROM_SOURCE="none"

# Optionally install the cmake for vcpkg
COPY ./reinstall-cmake.sh /tmp/
RUN if [ "${REINSTALL_CMAKE_VERSION_FROM_SOURCE}" != "none" ]; then \
        chmod +x /tmp/reinstall-cmake.sh && /tmp/reinstall-cmake.sh ${REINSTALL_CMAKE_VERSION_FROM_SOURCE}; \
    fi \
    && rm -f /tmp/reinstall-cmake.sh

# [Optional] Uncomment this section to install additional vcpkg ports.
# RUN su vscode -c "${VCPKG_ROOT}/vcpkg install <your-port-name-here>"

# [Optional] Uncomment this section to install additional packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libssl-dev libcurl4-openssl-dev libsasl2-dev liblz4-dev libzstd-dev libxml2-dev zlib1g-dev autoconf

RUN ln -s /usr/lib/x86_64-linux-gnu/pkgconfig/libxml-2.0.pc /usr/lib/x86_64-linux-gnu/pkgconfig/libxml2.pc

RUN pushd /tmp \
    && curl -O -L "https://github.com/NixOS/patchelf/archive/refs/tags/0.12.tar.gz" \
    && echo "3dca33fb862213b3541350e1da262249959595903f559eae0fbc68966e9c3f56  0.12.tar.gz" | shasum -c \
    && tar xz < "0.12.tar.gz" \
    && cd patchelf-0.12 \
    && ./bootstrap.sh \
    && ./configure \
    && make \
    && make check \
    && make install \
    && cd .. \
    && popd
