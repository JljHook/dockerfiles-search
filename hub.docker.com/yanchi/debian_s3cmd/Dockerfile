FROM debian:stable

MAINTAINER Yanchi

#設定時區
#RUN rm /etc/timezone \
#    && echo "Asia/Taipei" > /etc/timezone \
#    && chmod 644 /etc/timezone
ENV TZ=Asia/Taipei
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    && chmod 644 /etc/timezone

#執行apt-get更新套件，設定Cron工作排程器
RUN apt-get update \
    && apt-get install -y --no-install-recommends runit \
    && apt-get install -y --no-install-recommends vim \
    && apt-get install -y --no-install-recommends cron \
    && apt-get install -y --no-install-recommends python-pip \
    && apt-get install -y --no-install-recommends ssh \
    && mkdir /etc/service/cron \
    && echo '#!/bin/sh' > /etc/service/cron/run \
    && echo 'exec /usr/sbin/cron -f' >> /etc/service/cron/run \
    && chmod -R 700 /etc/service/cron/ \
    && chmod 600 /etc/crontab \
    && rm -f /etc/cron.daily/standard \
    && rm -f /etc/cron.daily/upstart \
    && rm -f /etc/cron.daily/dpkg \
    && rm -f /etc/cron.daily/password \
    && rm -f /etc/cron.weekly/fstrim \
    && apt-get purge -y --auto-remove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && pip install s3cmd 
    
	
#設定WorkDir
WORKDIR /etc
CMD ["runsv", "/etc/service/cron"]
