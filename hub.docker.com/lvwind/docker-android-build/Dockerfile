# Glibc included
FROM frolvlad/alpine-glibc:alpine-3.7

ENV ANDROID_APIS="android-28" \
    ANDROID_BUILD_TOOLS_VERSION=28.0.0 \
    ANDROID_HOME="/opt/android" \
    ANDROID_SDK_URL="https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip"

ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/build-tools/${ANDROID_BUILD_TOOLS_VERSION}

RUN apk update && \
  apk upgrade && \
  apk --no-cache add \
    openjdk8 \
    ca-certificates \
    curl \
    openssl \
    bash \
    git \
    ruby \
    ruby-bundler \
    ruby-json \
    ruby-dev \
    make \
    gcc \
    libc-dev

# Install sdk tools
RUN mkdir -p ${ANDROID_HOME} && \
    curl -o sdk-tools.zip ${ANDROID_SDK_URL} && \
    unzip sdk-tools.zip -d ${ANDROID_HOME} && \
    rm sdk-tools.zip && \
    yes | ${ANDROID_HOME}/tools/bin/sdkmanager --licenses

# Install sdk with sdk manager
RUN ${ANDROID_HOME}/tools/bin/sdkmanager \
    "platform-tools" \
    "platforms;${ANDROID_APIS}" \
    "build-tools;${ANDROID_BUILD_TOOLS_VERSION}" \
    "extras;android;m2repository"

# Setup environment.
ENV JAVA_HOME="/usr/lib/jvm/java-1.8-openjdk"

# Go to workspace.
WORKDIR /opt/workspace