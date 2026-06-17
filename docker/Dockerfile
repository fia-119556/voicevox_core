FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# ---- packages ----
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    unzip \
    zip \
    build-essential \
    pkg-config \
    cmake \
    ninja-build \
    python3 \
    python3-pip \
    clang \
    lld \
    llvm \
    openjdk-17-jdk \
    && rm -rf /var/lib/apt/lists/*

# =========================
# Java
# =========================
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV PATH=$JAVA_HOME/bin:$PATH

# =========================
# Android SDK
# =========================
ENV ANDROID_SDK_ROOT=/opt/android-sdk

RUN mkdir -p /opt/android-sdk/cmdline-tools && \
    cd /opt/android-sdk/cmdline-tools && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip && \
    unzip commandlinetools-linux-*.zip && \
    mv cmdline-tools latest && \
    rm commandlinetools-linux-*.zip

ENV PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH

RUN yes | sdkmanager --licenses && \
    sdkmanager \
      "platform-tools" \
      "platforms;android-21" \
      "build-tools;30.0.3"

# =========================
# NDK
# =========================
ENV ANDROID_NDK_VERSION=r27d
ENV ANDROID_NDK_ROOT=/opt/android-ndk
ENV ANDROID_NDK_HOME=/opt/android-ndk
ENV TOOLCHAIN=$ANDROID_NDK_HOME/toolchains/llvm/prebuilt/linux-x86_64/bin

ENV CC=$TOOLCHAIN/aarch64-linux-android24-clang
ENV CXX=$TOOLCHAIN/aarch64-linux-android24-clang++
ENV AR=$TOOLCHAIN/llvm-ar

RUN mkdir -p /opt && cd /opt && \
    wget https://dl.google.com/android/repository/android-ndk-${ANDROID_NDK_VERSION}-linux.zip && \
    unzip android-ndk-${ANDROID_NDK_VERSION}-linux.zip && \
    mv android-ndk-${ANDROID_NDK_VERSION} android-ndk && \
    rm android-ndk-${ANDROID_NDK_VERSION}-linux.zip

ENV PATH=$ANDROID_NDK_ROOT:$PATH

# =========================
# Rust
# =========================
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
    | sh -s -- -y

ENV PATH=/root/.cargo/bin:$PATH

WORKDIR /workspace


CMD ["/bin/bash"]
