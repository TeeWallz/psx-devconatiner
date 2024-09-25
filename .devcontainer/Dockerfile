ARG VARIANT=ubuntu-22.04
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

# Initial setup
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    && apt-get -y install build-essential git p7zip-full wget curl unzip sudo software-properties-common ninja-build gdb-multiarch \
    && apt-get clean

# PSN00bSDK
# Install CMake
RUN apt update && \
    apt install -y software-properties-common lsb-release && \
    wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | sudo tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null && \
    apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main" && \
    apt update && \
    apt install kitware-archive-keyring && \
    rm /etc/apt/trusted.gpg.d/kitware.gpg && \
    apt install -y cmake && \
    apt clean all

# Download and extract PSN00bSDK to /opt/psn00bsdk/bin
RUN cd /tmp && \
    wget https://github.com/Lameguy64/PSn00bSDK/releases/download/v0.24/PSn00bSDK-0.24-Linux.zip && \
    unzip PSn00bSDK-0.24-Linux.zip && \
    rm PSn00bSDK-0.24-Linux.zip && \
    mv PSn00bSDK-0.24-Linux /opt/psn00bsdk

# Generate ssh keys
RUN ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa -N ""

# Add PSN00bSDK to PATH
ENV PSN00B_DIR="/opt/psn00bsdk"
ENV PATH="$PSN00B_DIR/bin:${PATH}"
ENV PSN00BSDK_LIBS="$PSN00B_DIR/lib/libpsn00b"
ENV CMAKE_TOOLCHAIN_FILE="$PSN00B_DIR/lib/libpsn00b/cmake/sdk.cmake "

# Clone and build PSn00bSdk sample project
RUN mkdir -p /opt/sample_projects/psn00bsdk/ && \
    cp -r /opt/psn00bsdk/share/psn00bsdk/examples/* /opt/sample_projects/psn00bsdk/&& \
    echo '{ \n\
  "version": 3,\n\
  "cmakeMinimumRequired": {\n\
    "major": 3,\n\
    "minor": 21,\n\
    "patch": 0\n\
  },\n\
  "configurePresets": [\n\
    {\n\
      "name":          "default",\n\
      "displayName":   "Default configuration",\n\
      "description":   "Use this preset to build the project using PSn00bSDK.",\n\
      "generator":     "Ninja",\n\
      "toolchainFile": "$env{PSN00BSDK_LIBS}/cmake/sdk.cmake",\n\
      "binaryDir":     "${sourceDir}/build",\n\
      "cacheVariables": {\n\
        "CMAKE_BUILD_TYPE": "Debug",\n\
        "PSN00BSDK_TC":     "",\n\
        "PSN00BSDK_TARGET": "mipsel-none-elf"\n\
      },\n\
      "warnings": {\n\
        "dev": false\n\
      }\n\
    }\n\
  ]\n\
}' > /opt/sample_projects/psn00bsdk/CMakePresets.json &&\
  cd /opt/sample_projects/psn00bsdk/ && \
  cmake --preset default . && \
  cmake --build ./build

# Lightweight sample PSN00bSDK project
RUN mkdir -p /opt/sample_projects/psn00bsdk_new_project && \
    cp -r /opt/psn00bsdk/share/template/* /opt/sample_projects/psn00bsdk_new_project/ && \
    cd /opt/sample_projects/psn00bsdk_new_project/ && \
  cmake --preset default . && \
  cmake --build ./build

WORKDIR /workspace