FROM ubuntu:latest

RUN apt-get update
RUN apt-get install wget software-properties-common -y
RUN add-apt-repository ppa:vbernat/haproxy-1.5
RUN apt-get update
RUN apt-get install haproxy=1.5.14-1ppa~trusty -y
RUN rm -rf /var/lib/apt/lists/*

RUN wget -O /usr/local/bin/confd https://github.com/kelseyhightower/confd/releases/download/v0.7.1/confd-0.7.1-linux-amd64
RUN chmod +x /usr/local/bin/confd
RUN mkdir -p /etc/confd/{conf.d,templates}

ADD confd/haproxy.toml /etc/confd/conf.d/haproxy.toml
ADD confd/haproxy.cfg.tmpl /etc/confd/templates/haproxy.cfg.tmpl

ADD default/haproxy /etc/default/haproxy
ADD scripts/start /start

EXPOSE 80
EXPOSE 9000

CMD ["/start"]
