FROM nocturnalcm/oscore-bionic:latest
MAINTAINER "nocturnal coding monkeys,llc"

# ENVs
ENV TERM="xterm"
ENV LANG="C.UTF-8"
ENV LC_ALL="C.UTF-8"

# ARGs
ARG DEBIAN_FRONTEND="noninteractive"
ARG MONO_PACKAGES="bzip2 ca-certificates-mono libcurl4-openssl-dev mediainfo mono-devel mono-vbnc python sqlite3 unzip "

RUN \
 echo "**** install apt-transport-https ****" && \
 apt-get update && \
 apt-get install -y --no-install-recommends --no-install-suggests apt-transport-https gnupg2 && \
 echo "**** add mono repository ****" && \
 apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
 echo "deb http://download.mono-project.com/repo/ubuntu bionic main" | tee /etc/apt/sources.list.d/mono-official.list && \
 echo "**** add mediaarea repository ****" && \
 apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5CDF62C7AE05CC847657390C10E11090EC0E438 && \
 echo "deb https://mediaarea.net/repo/deb/ubuntu bionic main" | tee /etc/apt/sources.list.d/mediaarea.list && \
 echo "**** install packages ****" && \
 apt-get update && \
 apt-get install -y \
	--no-install-recommends \
	--no-install-suggests \
	${MONO_PACKAGES} && \
 echo "**** clean up ****" && \
 rm -rf \
	/tmp/* \
	/var/lib/apt/lists/* \
	/var/tmp/*
