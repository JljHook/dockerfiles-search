FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive
ENV USER root
ENV TZ Asia/Shanghai
ENV LANG zh_CN.UTF-8

RUN locale-gen zh_CN.UTF-8 \
&& apt-get update \
&& apt-get install -y --no-install-recommends ubuntu-desktop \
&& apt-get install -y gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal \
&& apt-get install -y tightvncserver \
&& apt-get install -y openssh-server supervisor vim git firefox firefox-locale-zh-hans ttf-wqy-microhei libnet1-dev libpcap0.8-dev \
&& apt-get install -y language-pack-zh-hans-base language-pack-zh-hans language-pack-gnome-zh-hans language-pack-gnome-zh-hans-base \
&& mkdir /root/.vnc \
&& mkdir /var/run/sshd \
&& echo 'root:root' |chpasswd \
&& sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config \
&& sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config \
&& apt-get clean \

COPY supervisord.conf /etc/supervisord.conf
COPY reset.sh /root/reset.sh
COPY check.sh /root/check.sh
COPY vnc.sh /root/.vnc/vnc.sh

RUN git clone https://github.com/snooda/net-speeder.git net-speeder
WORKDIR net-speeder
RUN sh build.sh \
&& mv net_speeder /usr/local/bin/

ADD xstartup /root/.vnc/xstartup
ADD passwd /root/.vnc/passwd

ADD entrypoint.sh /usr/sbin
RUN chmod 600 /root/.vnc/passwd && \
    chmod +x /usr/local/bin/net_speeder
WORKDIR /root

EXPOSE 22 5901 443 80
ENTRYPOINT ["entrypoint.sh"]

