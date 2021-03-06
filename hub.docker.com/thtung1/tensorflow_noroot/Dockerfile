FROM tensorflow/tensorflow:latest-devel-gpu-py3
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

WORKDIR /home

ENV OPENCV_VERSION='4.1.1'

# Install dependencies
RUN \
    # disable silly asking questions during installation
    DEBIAN_FRONTEND=noninteractive && \

    # just for the package libjasper-dev in Media I/O part
    add-apt-repository 'deb http://security.ubuntu.com/ubuntu xenial-security main' && \

    # Uncomment if you want to build this image yourself
    TZ=Asia/Ho_Chi_Minh && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \

    # Update Ubuntu source files
    apt update && \

    # You should never upgrade!!!
    # apt upgrade -y && \

    # Build tools
    apt install -y build-essential cmake curl git && \

    # GUI
    apt install -y qt5-default libvtk6-dev && \

    # Media I/O
    apt install -y zlib1g-dev libjpeg-dev libwebp-dev libpng-dev libtiff5-dev libjasper-dev libopenexr-dev libgdal-dev && \

    # Video I/O
    apt install -y libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev yasm libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev libxine2-dev && \

    # Parallelism and linear algebra libraries
    apt install -y libtbb-dev libeigen3-dev && \

    # support to HDF5
    apt install -y libhdf5-dev && \

    # Python
    apt install -y python-dev python-tk python-numpy python3-dev python3-tk python3-numpy && \

    # Java
    apt install -y ant default-jdk && \

    # Documentation:
    apt install -y doxygen && \

    # sudo
    apt install -y sudo && \

    # try nodejs 12
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt install -y nodejs && \

    # Cleanup
    apt autoclean && apt clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Jupyter lab
RUN \
    pip install jupyterlab matplotlib scipy jupyter_contrib_nbextensions yapf ipywidgets ipympl && \

    # Extensions for jupyterlab and notebook
    # for notebook localhost:8889/tree
    jupyter contrib nbextension install && \
    jupyter nbextension enable hinterland/hinterland && \

    # For jupyterlab localhost:8889/lab
    # interactive HTML
    jupyter nbextension enable --py --sys-prefix widgetsnbextension && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager && \

    #jupyter matplotlib: add this to the notebook: %matplotlib widget
    jupyter labextension install @jupyter-widgets/jupyterlab-manager && \
    jupyter labextension install jupyter-matplotlib && \
    
    # Go to definition (Ctrl + Alt + B keyboard-only)
    jupyter labextension install @krassowski/jupyterlab_go_to_definition

# Download OpenCV and install
RUN \
    # Download OpenCV and OpenCV-contrib
    mkdir -p opencv/build && \
    curl -L https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv --strip-components 1 && \
    mkdir opencv_contrib && \
    curl -L https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv_contrib --strip-components 1 && \
    cd opencv/build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D WITH_QT=ON \
    -D WITH_CUDA=OFF \
    -D WITH_OPENGL=ON \
    -D FORCE_VTK=ON \
    -D WITH_TBB=ON \
    -D WITH_GDAL=ON \
    -D WITH_XINE=ON \
    -D BUILD_EXAMPLES=OFF \
    -D ENABLE_PRECOMPILED_HEADERS=OFF \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_TESTS=OFF \
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules/ .. && \
    make -j $(getconf _NPROCESSORS_ONLN) && \
    make install && \
    ldconfig

# Download and install Keras-tuner
RUN \
    git clone https://github.com/keras-team/keras-tuner.git && \
    cd keras-tuner && \
    sed -i 's/tensorflow/tensorflow-gpu/g' setup.py && \
    pip install .

# To use TAB to complete commands
ENV SHELL=/bin/bash
