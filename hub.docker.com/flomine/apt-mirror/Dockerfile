FROM debian:stretch-slim

# Install apt-mirror and save some space by removing the apt-cache
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y apt-mirror nginx multitail cron && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD apt-mirror.lst /etc/apt/mirror.list
ADD nginx-bionic-mirror.conf etc/nginx/sites-enabled/default
ADD cron-apt-mirror /etc/cron.d/apt-mirror
ADD run.sh /run.sh

# This volume is where the mirrored data will be stored.  
VOLUME /var/spool/apt-mirror
VOLUME /var/log
EXPOSE 80
CMD /run.sh
