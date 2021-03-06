FROM nvidia/cuda:8.0-cudnn6-devel
MAINTAINER "Abhinav K"

COPY ./keyboard /etc/default/keyboard

# Based on https://github.com/OpenKinect/libfreenect2
# and https://hub.docker.com/r/nucleardreamer/libfreenect2/
## Uncomment libglfw3-dev if you want to install OpenGL
## Uncomment beignet-dev opencl-headers if you want to install OpenCL
## Uncomment libva-dev libjpeg-dev if you want to install Intel VAAPI
RUN apt-get update && apt-get install -yq \
    build-essential cmake pkg-config \
    software-properties-common \
    && add-apt-repository -y ppa:graphics-drivers/ppa \
    && apt-get update \
    && apt-get install -y --no-install-recommends nvidia-opencl-icd-384 \
    && apt-get install -yq libusb-1.0.0-dev \
    libturbojpeg libjpeg-turbo8-dev \
    libglfw3-dev \
    beignet-dev \
    nvidia-modprobe opencl-headers clinfo \
    libva-dev libjpeg-dev \
    libopenni2-0 libopenni2-dev openni2-utils \
    udev \
    git \
    cuda-samples-$CUDA_PKG_VERSION \
    xvfb \
    && rm -rf /var/lib/apt/lists/*

# Build freenect2 from source
# TODO: Checkout stable version
RUN git clone https://github.com/OpenKinect/libfreenect2.git \
    && cd libfreenect2 \
    && mkdir build \
    && cd build \
    && cmake .. -DBUILD_OPENNI2_DRIVER=ON -DCMAKE_INSTALL_PREFIX=/root/freenect2 \
    && make -j4 \
    && make -j4 install \
    && cp ../platform/linux/udev/90-kinect2.rules /etc/udev/rules.d/ \
    && ldconfig /root/freenect2 \
    && ln -s /libfreenect2/build/bin/Protonect /usr/local/bin/kinect_test

# Based on http://wiki.ros.org/kinetic/Installation/Ubuntu
RUN apt-get update \
    && apt-get install -yq lsb-release \
    && sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' \
    && apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116 \
    && apt-get install -yq python-pip \
    && pip install setuptools \
    && apt-get update \
    && apt-get install -yq ros-kinetic-desktop-full \
    && apt-get update && apt-get install -yq \
        python-rosinstall python-rosinstall-generator \
        python-wstool build-essential \
    && mkdir -p /catkin_ws/src \
    && cd /catkin_ws \
    && echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc \
    && /bin/bash -c "source /opt/ros/kinetic/setup.bash; catkin_make" \
    && rosdep init \
    && rosdep update \
    && apt-get install -yq  --reinstall ros-kinetic-vision-opencv \
        ros-kinetic-cv-bridge ros-kinetic-compressed-depth-image-transport \
        ros-kinetic-image-transport-plugins ros-kinetic-perception-pcl \
        ros-kinetic-perception ros-kinetic-message-runtime \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --upgrade pip \
    && pip install redis flask flask-restful shapely
