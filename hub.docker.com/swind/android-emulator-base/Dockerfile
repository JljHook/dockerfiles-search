FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

# ================================================
#
# Gerneral packages
#
# ================================================
RUN ln -fs /usr/share/zoneinfo/Asia/Taipei /etc/localtime \
&& dpkg --add-architecture i386 \
&& apt-get update \
&& apt-get install -y --no-install-recommends libncurses5:i386 libc6:i386 libstdc++6:i386 lib32gcc1 lib32ncurses5 lib32z1 zlib1g:i386 \
&& apt-get install -y --no-install-recommends file git curl unzip zip sudo\
&& apt-get install -y --no-install-recommends openjdk-8-jdk \
&& apt-get install -y --no-install-recommends supervisor \
&& apt-get install -y libqt5widgets5 socat net-tools \
&& apt-get install -y xterm x11vnc openbox xvfb \
&& apt-get install -y ffmpeg \
&& apt-get install -y qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils tzdata \
&& dpkg-reconfigure --frontend noninteractive tzdata \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists /var/cache/apt

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

RUN mkdir -p /root
WORKDIR /root

# ================================================
#
# Android SDK
#
# ================================================
ENV ANDROID_HOME="/root/android-sdk-linux"

# Download Android SDK
ARG ANDROID_SDK_VERSION=4333796
ARG ANDROID_SDK_URL=https://dl.google.com/android/repository/sdk-tools-linux-${ANDROID_SDK_VERSION}.zip

RUN mkdir "$ANDROID_HOME" .android \
 && cd "$ANDROID_HOME" \
 && curl -o sdk.zip $ANDROID_SDK_URL \
 && unzip sdk.zip \
 && rm sdk.zip

ENV PATH="${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools:${PATH}"

 # accept the license agreements of the SDK components
ADD scripts/license_accepter.sh /root/android-sdk-linux
RUN $ANDROID_HOME/license_accepter.sh $ANDROID_HOME

#================================================
# noVNC Default Configurations
# These Configurations can be changed through -e
#================================================
ENV DISPLAY=:0 \
    SCREEN=0 \
    SCREEN_WIDTH=1600 \
    SCREEN_HEIGHT=900 \
    SCREEN_DEPTH=24+32 \
    LOCAL_PORT=5900 \
    TARGET_PORT=6080 \
    TIMEOUT=1 \
    VIDEO_PATH=/tmp/video \
    LOG_PATH=/var/log/supervisor

# ==============================================================================================================
#
# noVNC
# Use same commit id that docker-selenium uses
# https://github.com/elgalu/docker-selenium/blob/236b861177bd2917d864e52291114b1f5e4540d7/Dockerfile#L412-L413
#
# ==============================================================================================================

ENV NOVNC_SHA="b403cb92fb8de82d04f305b4f14fa978003890d7" \
    WEBSOCKIFY_SHA="558a6439f14b0d85a31145541745e25c255d576b"

RUN  wget -nv -O noVNC.zip "https://github.com/kanaka/noVNC/archive/${NOVNC_SHA}.zip" \
 && unzip -x noVNC.zip \
 && rm noVNC.zip  \
 && mv noVNC-${NOVNC_SHA} noVNC \
 && wget -nv -O websockify.zip "https://github.com/kanaka/websockify/archive/${WEBSOCKIFY_SHA}.zip" \
 && unzip -x websockify.zip \
 && mv websockify-${WEBSOCKIFY_SHA} ./noVNC/utils/websockify \
 && rm websockify.zip \
 && ln noVNC/vnc_auto.html noVNC/index.html

# ================================================
#
# Copy config files
#
# ================================================

ADD configs/sources1810.list /etc/apt/sources.list.d/
ADD configs/x11vnc.pref /etc/apt/preferences.d/

ADD scripts/.emulator_console_auth_token /root
ADD scripts/socat.sh /root
ADD scripts/supervisord.conf /root
ADD scripts/entrypoint.sh /root

# ================================================
#
# Start supervisord and emualtor
#
# ================================================
EXPOSE 5554 5555
ENTRYPOINT ["/root/entrypoint.sh"]
CMD ["/usr/bin/supervisord"]
