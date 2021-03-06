FROM nvidia/cuda:8.0-cudnn7-devel-ubuntu16.04

LABEL maintainer="Jingpeng Wu"\
        project="kaffe"

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        git \
        wget \
        libatlas-base-dev \
        libboost-all-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        python-dev \
        python-numpy \
        python-pip && \
    rm -rf /var/lib/apt/lists/*
RUN pip install --upgrade pip
RUN pip install -U setuptools 
RUN pip install scipy pydot 

# correct the library name
WORKDIR /usr/lib/x86_64-linux-gnu
RUN ln -s libhdf5_serial.so.8.0.2 libhdf5.so && \
    ln -s libhdf5_serial_hl.so.8.0.2 libhdf5_hl.so
ENV CPATH /usr/include/hdf5/serial/


# download and install caffe
WORKDIR /opt 
ENV CAFFE_ROOT=/opt/caffe
ENV CLONE_TAG=master
RUN git clone -b ${CLONE_TAG} --depth 1 https://github.com/jingpengw/caffe.git
WORKDIR $CAFFE_ROOT
RUN pip install -r ${CAFFE_ROOT}/python/requirements.txt


#RUN apt-get install -y --no-install-recommends libopencv-dev 
COPY Makefile.config ./  
RUN make clean
#RUN make -j "$(nproc)" all 
RUN make -j "$(nproc)" pycaffe

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

WORKDIR $CAFFE_ROOT
