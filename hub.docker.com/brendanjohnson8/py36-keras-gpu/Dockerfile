# FROM defines the base image
FROM gcr.io/tensorflow/tensorflow:latest-gpu-py3
MAINTAINER Brendan Johnson <brendan@neap.co>

# Disable interactive interface
ARG DEBIAN_FRONTEND=noninteractive

# RUN executes a shell command
# You can chain multiple commands together with &&
# A \ is used to split long lines to help with readability
RUN apt-get update && apt-get install -y \
    # tools
    git \
    vim \
    python-setuptools \
    python-virtualenv \
    graphviz

# Install python 3.6 port
RUN : \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        software-properties-common \
    && add-apt-repository ppa:deadsnakes \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        python3.6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install keras and theano dependencies not included in
# https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/docker/Dockerfile.gpu
RUN pip install -U pip && pip --no-cache-dir install --upgrade \
    pandas \
    h5py \
    pyyaml \
    virtualenv \
    graphviz \
    pydot \
    keras-tqdm

# Enable the widgetsnbextension for notebook
RUN jupyter nbextension enable --py --sys-prefix widgetsnbextension

# Install theano and keras
RUN pip --no-cache-dir install --no-deps \
    theano \
    keras \
    git+https://www.github.com/farizrahman4u/keras-contrib.git

# Install PIP for Python 3.6
ADD https://bootstrap.pypa.io/get-pip.py /tmp/
RUN python3.6 /tmp/get-pip.py

# Set keras backend to tensorflow by default
ENV KERAS_BACKEND tensorflow

# Add /root/workspace to PYTHONPATH for convenience
ENV PYTHONPATH /root/workspace:$PYTHONPATH

# Set the working directory
WORKDIR /root/workspace

# CMD defines the default command to be run in the container
# CMD is overridden by supplying a command + arguments to
# `docker run`, e.g. `nvcc --version` or `bash`
CMD ["/run_jupyter.sh"]
