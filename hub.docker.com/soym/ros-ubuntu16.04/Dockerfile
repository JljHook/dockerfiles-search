FROM ubuntu:xenial

# install packages
RUN apt-get update && apt-get install -q -y \
    dirmngr \
    gnupg2 \
    lsb-release \
    && rm -rf /var/lib/apt/lists/*

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 421C365BD9FF1F717815A3895523BAEEB01FA116

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list

# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    python-rosdep \
    python-rosinstall \
    python-vcstools \
    && rm -rf /var/lib/apt/lists/*

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# bootstrap rosdep
RUN rosdep init \
    && rosdep update

# install ros packages
ENV ROS_DISTRO kinetic
RUN apt-get update && apt-get install -y \
    ros-kinetic-ros-base=1.3.2-0* \
    && rm -rf /var/lib/apt/lists/*
COPY ./install/ /install/

# setup entrypoint
ENTRYPOINT ["/install/entrypoint.sh"]

CMD ["bash"]


# FROM ros:kinetic-ros-base-xenial
# COPY ./install/ /root/install/

# RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc \
# && echo "source ~/install/setup.bash" >> ~/.bashrc && /bin/bash -c "source ~/.bashrc" 

# ENTRYPOINT ["~/install/entrypoint.sh"]
