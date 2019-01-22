FROM alpine:3.8

RUN apk add --no-cache python3 python3-dev gcc libffi-dev openssl-dev musl-dev make && \
python3 -m ensurepip && \
rm -r /usr/lib/python*/ensurepip && \
pip3 install --upgrade pip setuptools && \
if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
if [[ ! -e /usr/bin/python ]]; then ln -sf /usr/bin/python3 /usr/bin/python; fi && \
pip3 install fabric==2.4 && \
apk del python3-dev gcc libffi-dev openssl-dev musl-dev make && \
rm -r /root/.cache

WORKDIR /app

VOLUME /app
