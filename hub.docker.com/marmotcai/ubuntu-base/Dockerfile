FROM ubuntu:16.04 AS base
MAINTAINER marmotcai "marmotcai@163.com"

ARG PROXY=""
ARG CONFIGDIR=""

# RUN if [ ${PROXY} = "" ]; then \
ENV ALL_PROXY sock5://${PROXY}
# ;fi

ENV DEBIAN_FRONTEND=noninteractive \
    TERM=xterm

# ADD $CONFIGDIR/sources.list /etc/apt/

# RUN rm -f /etc/apt/sources.list && \
#	echo "deb http://mirrors.aliyun.com/ubuntu/ xenial main" >> /etc/apt/sources.list && \
#	echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial main" >> /etc/apt/sources.list && \
#	echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main" >> /etc/apt/sources.list && \
#	echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main" >> /etc/apt/sources.list && \
#	echo "deb http://mirrors.aliyun.com/ubuntu/ xenial universe" >> /etc/apt/sources.list && \
#	echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe" >> /etc/apt/sources.list && \
#	echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe" >> /etc/apt/sources.list && \
#	echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe" >> /etc/apt/sources.list && \
#	echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-security main" >> /etc/apt/sources.list && \
#	echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main" >> /etc/apt/sources.list && \
#	echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe" >> /etc/apt/sources.list && \
#	echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe" >> /etc/apt/sources.list

# RUN cat /etc/apt/sources.list

RUN apt-get update && \
    apt-get install -y wget vim openssh-server && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    apt-get autoremove -y && \
    apt-get clean

RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config && \
	sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/g' /etc/ssh/sshd_config && \
	sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config && \
	echo "root:112233" | chpasswd && \
	mkdir /var/run/sshd

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
