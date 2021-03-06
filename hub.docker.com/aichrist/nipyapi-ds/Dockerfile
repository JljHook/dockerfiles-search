FROM alpine:3.10.2
RUN apk add bash \
            curl \
            gzip \
            python3 \
            python3-dev \
            musl-dev \
            libffi-dev \
            libressl-dev \
            libxml2-dev \
            libxslt-dev \
            build-base \
            nodejs \
            npm \
            docker \
            wget

RUN wget -O /bin/kubectl "https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/linux/amd64/kubectl" && \
    chmod +x /bin/kubectl

RUN cd /opt && \
    wget -O - "https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-267.0.0-linux-x86_64.tar.gz" | \
    gunzip | \
    tar xv

RUN ln -s /opt/google-cloud-sdk/bin/gcloud /bin/gcloud
RUN ln -s /opt/google-cloud-sdk/bin/docker-credential-gcloud /bin/docker-credential-gcloud

RUN pip3 install -U pip
ADD requirements.txt /requirements.txt
RUN pip3 install -r /requirements.txt && rm /requirements.txt
ADD . /opt/nipyapi
RUN cd /opt/nipyapi/nipyapi/webui && \
    npm i && \
    npm run build
RUN mkdir -p /opt/nipyapi/bin
ADD nipyapi/webui/docker/start.sh /opt/nipyapi/bin/
VOLUME /opt/nipyapi/data
EXPOSE 8000
ENV DJANGO_SETTINGS_MODULE webui.settings_docker
CMD /opt/nipyapi/bin/start.sh
