FROM node

RUN apt-get update \
    && apt-get install -y \
    python3-pip \
    jq \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN pip3 install --upgrade awscli

