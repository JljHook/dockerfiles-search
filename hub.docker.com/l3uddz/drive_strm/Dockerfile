FROM python:3.6

LABEL maintainer="l3uddz"

ENV DRIVE_STRM_CONFIG_PATH=/config/config.json \
    DRIVE_STRM_TOKEN_PATH=/config/token.json \
    DRIVE_STRM_CACHE_PATH=/config/cache.db \
    DRIVE_STRM_LOG_PATH=/config/activity.log

WORKDIR /app

# clone project
RUN git clone https://github.com/l3uddz/drive_strm /app

# install requirements
RUN pip install -r requirements.txt

# volumes and ports
EXPOSE 7294
VOLUME ["/config", "/strm"]

# start
CMD ["python3", "drive_strm.py", "run"]