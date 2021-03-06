FROM ubuntu:18.04
MAINTAINER Taewan Kim <taewanme@gmail.com>

USER root


RUN apt-get update \
   &&  apt-get install -y curl apt-utils git make build-essential \
               libssl-dev libffi-dev zlib1g-dev libbz2-dev libreadline-dev   \
               libsqlite3-dev python3.6 python-pip python3-pip libjpeg8-dev python-dev \
               language-pack-ko python3-dev libxml2 libxml2-dev libxslt1-dev \
               fonts-nanum fonts-nanum-coding fonts-nanum-extra \
               cmake git wget unzip yasm pkg-config libswscale-dev \
               libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libavformat-dev libpq-dev \
   && apt-get clean \
   && rm -rf /var/lib/apt/lists/*

ENV MV 8
ENV UV 181
ENV BV 13

ENV JDK_TAR_NAME jdk-${MV}u${UV}-linux-x64.tar.gz
ENV JAVA_DIR_NAME jdk1.${MV}.0_${UV}
ENV INSTALL_HOME /usr/local/java
ENV JAVA_HOME_DIR ${INSTALL_HOME}/${JAVA_DIR_NAME}
ENV DOWNLOAD_URL http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz
ENV JAVA_HOME ${JAVA_HOME_DIR}
ENV PATH $PATH:$MAVEN_HOME/bin:$JAVA_HOME/bin

RUN curl -b "oraclelicense=accept-securebackup-cookie" -L -C - -O ${DOWNLOAD_URL} && \
 mkdir -p ${INSTALL_HOME} && \
 tar xvfz ./${JDK_TAR_NAME} -C ${INSTALL_HOME} && \
 rm -f ./${JDK_TAR_NAME}

RUN update-alternatives --install "/usr/bin/java" "java" "${JAVA_HOME_DIR}/bin/java" 1 && \
 update-alternatives --install "/usr/bin/javac" "javac" "${JAVA_HOME_DIR}/bin/javac" 1 && \
 update-alternatives --install "/usr/bin/javaws" "javaws" "${JAVA_HOME_DIR}/bin/javaws" 1

# set locale ko_KR
RUN locale-gen ko_KR.UTF-8
ENV LANG ko_KR.UTF-8
ENV LANGUAGE ko_KR.UTF-8
ENV LC_ALL ko_KR.UTF-8

RUN python3 -m pip install --upgrade pip

RUN pip3 install xlrd            \
 && pip3 install numpy          \
 && pip3 install pillow         \
 && pip3 install matplotlib     \
 && pip3 install scikit-learn   \
 && pip3 install Pandas         \
 && pip3 install scrapy         \
 && pip3 install NLTK           \
 && pip3 install bokeh          \
 && pip3 install NetworkX       \
 && pip3 install scipy          \
 && pip3 install Seaborn        \
 && pip3 install jupyter         \
 && pip3 install beautifulsoup4  \
 && pip3 install keras           \
 && pip3 install http://download.pytorch.org/whl/cpu/torch-0.4.1-cp36-cp36m-linux_x86_64.whl \
 && pip3 install torchvision    \
 && pip3 install --upgrade tensorflow \
 && pip3 install jpype1 \
 && pip3 install konlpy==0.4.4 \
 && pip3 install torchtext \
 && pip3 install mglearn

WORKDIR /
ENV OPENCV_VERSION="3.4.3"
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
&& unzip ${OPENCV_VERSION}.zip \
&& mkdir /opencv-${OPENCV_VERSION}/cmake_binary \
&& cd /opencv-${OPENCV_VERSION}/cmake_binary \
&& cmake -DBUILD_TIFF=ON \
  -DBUILD_opencv_java=OFF \
  -DWITH_CUDA=OFF \
  -DWITH_OPENGL=ON \
  -DWITH_OPENCL=ON \
  -DWITH_IPP=ON \
  -DWITH_TBB=ON \
  -DWITH_EIGEN=ON \
  -DWITH_V4L=ON \
  -DBUILD_TESTS=OFF \
  -DBUILD_PERF_TESTS=OFF \
  -DCMAKE_BUILD_TYPE=RELEASE \
  -DCMAKE_INSTALL_PREFIX=$(python3.6 -c "import sys; print(sys.prefix)") \
  -DPYTHON_EXECUTABLE=$(which python3) \
  -DPYTHON_INCLUDE_DIR=$(python3.6 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
  -DPYTHON_PACKAGES_PATH=$(python3.6 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
  .. \
&& make install \
&& rm /${OPENCV_VERSION}.zip \
&& rm -r /opencv-${OPENCV_VERSION}

RUN mkdir -p /root/.jupyter/
COPY jupyter_notebook_config.py /root/.jupyter/
COPY start-notebook.sh /usr/local/bin

EXPOSE 8888
EXPOSE 6006

VOLUME ["/root/ipython"]
WORKDIR /root

ENTRYPOINT ["/bin/bash", "start-notebook.sh"]
