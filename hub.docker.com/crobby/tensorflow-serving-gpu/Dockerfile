# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM nvidia/cuda:8.0-cudnn6-devel-centos7

MAINTAINER Jingtian Peng <pjt73651@gmail.com>

USER root
RUN yum --enablerepo=extras install -y epel-release && \
    yum update -y && yum install -y \
        build-essential \
        curl \
        git \
        gcc \
        gcc-c++ \
        mlocate \
        patch \
        python-devel \
        python-pip \
        swig \
        zip \
        wget \
        unzip \
        which \
        && \
    yum clean all

# Set up grpc
ENV LD_LIBRARY_PATH=/opt/rh/python27/root/usr/lib64:/usr/local/cuda/lib64
RUN pip install --upgrade pip
RUN pip install mock grpcio numpy

# Set up all envs
ENV BAZELRC=/root/.bazelrc \
    TF_SERVING_VERSION=tags/1.4.0 \
    TF_CUDA_VERSION=8.0 \
    TF_CUDNN_VERSION=6 \
    BAZEL_VERSION=0.5.4 \
    TF_NEED_CUDA=1 \
    TF_NEED_S3=0 \
    TF_CUDA_COMPUTE_CAPABILITIES="3.5,5.2,6.1" \
    TF_NEED_GCP=1 \
    TF_NEED_JEMALLOC=0 \
    TF_NEED_HDFS=1 \
    TF_NEED_OPENCL=0 \
    TF_NEED_MKL=0 \
    TF_NEED_VERBS=0 \
    TF_NEED_MPI=0 \
    TF_NEED_GDR=0 \
    TF_ENABLE_XLA=1 \
    TF_CUDA_CLANG=0 \
    TF_NEED_OPENCL_SYCL=0 \
    CUDA_TOOLKIT_PATH=/usr/local/cuda \
    CUDNN_INSTALL_PATH=/usr/local/cuda \
    GCC_HOST_COMPILER_PATH=/usr/bin/gcc \
    PYTHON_BIN_PATH=/usr/bin/python \
    CC_OPT_FLAGS="-march=native" \
    PYTHON_LIB_PATH=/usr/lib64/python2.7/site-packages

# Install the most recent bazel release.
RUN mkdir /bazel && \
    cd /bazel && \
    curl -fSsL -O https://github.com/bazelbuild/bazel/releases/download/$BAZEL_VERSION/bazel-$BAZEL_VERSION-installer-linux-x86_64.sh && \
    curl -fSsL -o /bazel/LICENSE.txt https://raw.githubusercontent.com/bazelbuild/bazel/master/LICENSE && \
    chmod +x bazel-*.sh && \
    ./bazel-$BAZEL_VERSION-installer-linux-x86_64.sh && \
    rm -f /bazel/bazel-$BAZEL_VERSION-installer-linux-x86_64.sh

# Clone TensorFlow Serving repo and get submodules of TensorFlow Serving 1.4
RUN cd /root && \
    git clone --recurse-submodules https://github.com/tensorflow/serving && \
    cd serving && \
    git checkout $TF_SERVING_VERSION && \
    git submodule init && \
    git submodule update --recursive

# Configure Tensorflow to use the GPU, the touch stropts.h line is a workaround for a bug
RUN cd /root/serving/tensorflow && ./configure && \
    touch /usr/include/stropts.h

# Compile TF serving with CUDA support
RUN cd /root/serving && \
    bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-mfpmath=both --copt=-msse4.2 --config=cuda -k --verbose_failures --crosstool_top=@local_config_cuda//crosstool:toolchain tensorflow_serving/model_servers:tensorflow_model_server

# Add some softlinks for tensorflow model server and CUDA
RUN ln -s /root/serving/bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server /usr/bin/tensorflow_model_server

WORKDIR /root

CMD ["/bin/bash"]

ENV TINI_VERSION v0.17.0

ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini

RUN chmod +x /tini

ENTRYPOINT ["/tini", "--"]
