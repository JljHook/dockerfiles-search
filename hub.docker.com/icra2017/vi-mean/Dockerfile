FROM ros:indigo-perception

RUN apt-get update && apt-get install -y \
	python-catkin-pkg python-rosdep python-wstool \
	python-catkin-tools ros-indigo-catkin \
	&& rm -rf /var/lib/apt/lists

ENV CATKIN_WS=/root/catkin_ws
RUN rm /bin/sh \
	&& ln -s /bin/bash /bin/sh	

RUN apt-get update && apt-get install -y \
	build-essential software-properties-common \
	doxygen autoconf \
	&& rm -rf /var/lib/apt/lists	

RUN source /ros_entrypoint.sh \
	&& mkdir -p $CATKIN_WS/src \
	&& cd $CATKIN_WS/src && git clone https://github.com/ICRA2017/VI-MEAN.git

RUN source /ros_entrypoint.sh \
	&& cd $CATKIN_WS \
	&& rosdep install -y --from-paths src --ignore-src --rosdistro indigo

RUN apt-get update && apt-get install -y \
	libeigen3-dev ceres-dev \
	&& rm -rf /var/lib/apt/lists	
	
RUN source /ros_entrypoint.sh \
	&& cd $CATKIN_WS \
	&& catkin_make
