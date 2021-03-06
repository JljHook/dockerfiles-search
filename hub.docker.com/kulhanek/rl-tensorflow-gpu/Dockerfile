# deepmind-lab package container
FROM tensorflow/tensorflow:latest-py3 as artefact-builder
RUN apt-get update && apt-get install -y curl openjdk-8-jdk git
RUN echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list & \
    curl https://bazel.build/bazel-release.pub.gpg | apt-key add - && \
    apt-get update && apt-get install bazel -y

RUN apt-get update && apt-get install -y \
  lua5.1 \
  liblua5.1-0-dev \
  libffi-dev \
  gettext \
  freeglut3-dev \
  libsdl2-dev \
  libosmesa6-dev \
  python3-dev \
  python3-numpy \
  realpath \
  build-essential \
  zip

RUN mkdir /toolbox && cd /toolbox &&  git clone https://github.com/jkulhanek/lab.git
RUN echo "#!/bin/bash\nrm /tmp/.X1-lock /tmp/.X11-unix/X1 \nvncserver :1 -randr -geometry 1280x800 -depth 24 && tail -F /root/.vnc/*.log" | tee /opt/vnc.sh
ENV DISPLAY :1

RUN cd /toolbox/lab && bazel build :deepmind_lab.so --define headless=osmesa --force_python=PY3 --host_force_python=PY3

# we will build the pip package
RUN cd /toolbox/lab && \
  bazel build -c opt python/pip_package:build_pip_package --force_python=PY3 --host_force_python=PY3 && \
  mkdir /artifact && \
  ./bazel-bin/python/pip_package/build_pip_package /artifact

RUN pip install /artifact/DeepMind_Lab-1.0-py3-none-any.whl

# Output container
FROM nvidia/cuda:9.0-base

# Pick up some TF dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cuda-command-line-tools-9-0 \
        cuda-cublas-9-0 \
        cuda-cufft-9-0 \
        cuda-curand-9-0 \
        cuda-cusolver-9-0 \
        cuda-cusparse-9-0 \
        curl \
        libcudnn7=7.2.1.38-1+cuda9.0 \
        libnccl2=2.2.13-1+cuda9.0 \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        rsync \
        software-properties-common \
        unzip \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y \
  lua5.1 \
  liblua5.1-0-dev \
  libffi-dev \
  gettext \
  freeglut3-dev \
  libsdl2-dev \
  libosmesa6-dev \
  python3 \
  python3-pip \
  python3-dev \
  python3-numpy \
  python3-tk \
  realpath \
  build-essential \
  zip \
  git

# Redirect display
RUN echo "#!/bin/bash\nrm /tmp/.X1-lock /tmp/.X11-unix/X1 \nvncserver :1 -randr -geometry 1280x800 -depth 24 && tail -F /root/.vnc/*.log" | tee /opt/vnc.sh
ENV DISPLAY :1

# Install package
COPY --from=artefact-builder /artifact/DeepMind_Lab-1.0-py3-none-any.whl /tmp/DeepMind_Lab-1.0-py3-none-any.whl

RUN umask 022
RUN pip3 install --upgrade tensorflow-gpu
RUN pip3 install matplotlib six
RUN pip3 install /tmp/DeepMind_Lab-1.0-py3-none-any.whl

# Install gym and other packages
RUN pip3 install gym && \
  pip3 install git+https://github.com/jkulhanek/gym-maze-env.git
