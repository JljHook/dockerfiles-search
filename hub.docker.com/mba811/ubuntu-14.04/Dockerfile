FROM ubuntu:14.04
MAINTAINER Tsong khapa <ztd811@yahoo.com>
ADD etc /etc
CMD /config/loop
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
	apt-get upgrade -y && \
	apt-get install -y ca-certificates inotify-tools vim pwgen supervisor unrar unzip wget && \
	apt-get clean && \
	echo -n > /var/lib/apt/extended_states
RUN mkdir /config /data
RUN useradd -u 500 core
ADD config /config
RUN chmod +x /config/loop
