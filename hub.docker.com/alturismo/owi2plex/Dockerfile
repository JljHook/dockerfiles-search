FROM alpine:latest
RUN apk update
RUN apk upgrade
RUN apk add --no-cache ca-certificates

MAINTAINER alturismo alturismo@gmail.com

# Extras
RUN apk add --no-cache curl

# Install Python3 and owi2plex
RUN apk add --no-cache python3 py3-pip libxml2 libxml2-dev
RUN apk add --update --no-cache g++ libxslt-dev python3-dev
RUN pip3 install lxml
RUN pip3 install owi2plex

# Timezone (TZ)
RUN apk update && apk add --no-cache tzdata
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Add Bash shell & dependancies
RUN apk add --no-cache bash busybox-suid su-exec

# Volumes
VOLUME /config
VOLUME /owi2plex

# Add xTeve and guide2go
ADD cronjob.sh /
ADD entrypoint.sh /
ADD sample_cron.txt /

# Add Fix
COPY ./owi2plex.py /usr/bin/owi2plex.py

# Set executable permissions
RUN chmod +x /entrypoint.sh
RUN chmod +x /cronjob.sh

# Entrypoint
ENTRYPOINT ["./entrypoint.sh"]
