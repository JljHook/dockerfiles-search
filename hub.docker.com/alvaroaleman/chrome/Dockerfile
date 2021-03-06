## Run Chrome in a container
#
# This container is based on jfrazelles work
# https://github.com/jfrazelle/dockerfiles
#
## Selinux must be either disabled or an additional policy
## is required to allow Docker to connect to the X11 socket
# sudo setenforce 0
#
# AUDIOGROUPID=$(getent group audio|cut -d':' -f3)
# VIDEGROUPID=$(getent group video|cut -d':' -f3)
# docker run -d \
#       --memory 3gb \
#       --net host \
#       -v /etc/localtime:/etc/localtime:ro \
#       -v /tmp/.X11-unix:/tmp/.X11-unix \
#       -e DISPLAY=unix$DISPLAY \
#       -v $HOME/Downloads:/Downloads \
#       -v /dev/shm:/dev/shm \
#       -v /etc/hosts:/etc/hosts \
#       -v $HOME/.config/google-chrome/:/data \
#       -u `/usr/bin/id -u` \
#       --device /dev/snd \
#       --device /dev/dri \
#       --device /dev/video0 \
#       --group-add $AUDIOGROUPID \
#       --group-add $VIDEGROUPID \
#       --name chrome \
#      alvaroaleman/chrome --user-data-dir=/data --force-device-scale-factor=1

# Base docker image
FROM debian:sid
MAINTAINER Alvaro Aleman

ADD https://dl.google.com/linux/direct/google-talkplugin_current_amd64.deb /src/google-talkplugin_current_amd64.deb

ADD https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb /src/google-chrome-stable_current_amd64.deb

# Install Chrome
RUN mkdir -p /usr/share/icons/hicolor && \
	apt-get update && apt-get install -y \
	ca-certificates \
	fonts-liberation \
	gconf-service \
	hicolor-icon-theme \
	libappindicator1 \
	libasound2 \
	libcanberra-gtk-module \
	libcurl3 \
	libexif-dev \
	libgconf-2-4 \
	libgl1-mesa-dri \
	libgl1-mesa-glx \
	libnspr4 \
	libnss3 \
	libpango1.0-0 \
	libv4l-0 \
	libxss1 \
	libxtst6 \
	wget \
	xdg-utils \
	--no-install-recommends && \
	dpkg -i '/src/google-chrome-stable_current_amd64.deb' && \
	dpkg -i '/src/google-talkplugin_current_amd64.deb' \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /src/*.deb \
  && mkdir /Downloads \
  && chmod 777 /Downloads

COPY local.conf /etc/fonts/local.conf
COPY asoundconf /etc/asound.conf

# Autorun chrome
ENTRYPOINT [ "/usr/bin/google-chrome" ]
CMD [ "--user-data-dir=/data" ]
