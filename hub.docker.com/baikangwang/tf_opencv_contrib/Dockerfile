FROM baikangwang/tf_opencv_contrib:cpu_3.5
MAINTAINER Baker Wang <baikangwang@hotmail.com>

# Serving port
ENV SERVING_PORT 9000
# Client port
ENV CLIENT_PORT 8080

# Serving port & client port
EXPOSE $SERVING_PORT $CLIENT_PORT

RUN apt update && \
    #
    # Prerequisites
    #
    apt install -y --no-install-recommends \
        # Build tools
        build-essential g++ \
        # Developer Essentials
        curl git wget zip unzip \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        software-properties-common \
        swig \
        zlib1g-dev \
        libcurl3-dev && \
    # Grpc
    pip3 install --no-cache-dir mock grpcio && \
    # pip3 install --no-cache-dir tensorflow-serving-api && \
    #
    # Clean up
    #
    apt-get clean && \
    apt autoremove && \
    rm -rf /var/lib/apt/lists/* && \
    #
    # Install Tensorflow serving 1.3.0
    #
    # Install using apt-get
    echo "deb [arch=amd64] http://storage.googleapis.com/tensorflow-serving-apt stable tensorflow-model-server tensorflow-model-server-universal" | tee /etc/apt/sources.list.d/tensorflow-serving.list && \
    curl https://storage.googleapis.com/tensorflow-serving-apt/tensorflow-serving.release.pub.gpg | apt-key add - && \
    apt update && apt install tensorflow-model-server && \
    apt upgrade tensorflow-model-server && \
    #
    # Clean up
    #
    apt-get clean && \
    apt autoremove && \
    rm -rf /var/lib/apt/lists/*

#
# tensorflow-serving-api
# just copy the built files in python2 to python3.5 packages since there hasn't been official package supporting python3
#
COPY tensorflow_serving_api-1.3.0 /usr/local/lib/python3.5/dist-packages/

CMD ["/bin/bash"]