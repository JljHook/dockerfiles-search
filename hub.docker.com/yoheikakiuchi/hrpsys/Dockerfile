### after ros_gl
# docker build -f <this_file> -t jsk_private/hrpsys:16.04 .
FROM yoheikakiuchi/choreonoid:latest

SHELL ["/bin/bash", "-c"]

ENV MY_ROSWORKSPACE /hrpsys_ws
#ENV HRPSYS_PARENT_SETUP /opt/ros/$ROS_DISTRO/setup.bash
ENV HRPSYS_PARENT_SETUP /choreonoid_ws/devel/setup.bash

## build (base) workspace
WORKDIR ${MY_ROSWORKSPACE}

RUN wstool init src && \
#    wstool set -y -t src hrpsys   https://github.com/fkanehiro/hrpsys-base.git --git && \
    wstool set -y -t src openhrp3 https://github.com/fkanehiro/openhrp3.git    --git && \
    wstool update -t src

COPY . src/hrpsys

RUN sed -i -e 's@"Use Doxygen" ON@"Use Doxygen" OFF@' src/hrpsys/CMakeLists.txt
RUN sed -i -e 's@"Use Doxygen" ON@"Use Doxygen" OFF@' src/openhrp3/CMakeLists.txt

## rosdep install
RUN source ${HRPSYS_PARENT_SETUP} && \
    apt-get update -q -qq && \
    rosdep install -q -y -r --from-paths src --ignore-src || echo 'Ignore rosdep error' && \
    apt-get install -q -qq -y libopencv-dev libsdl1.2-dev freeglut3-dev libglew-dev && \
    apt-get clean  && \
    rm -rf /var/lib/apt/lists/*

## build hrpsys
WORKDIR ${MY_ROSWORKSPACE}
RUN /bin/bash -c "source ${HRPSYS_PARENT_SETUP} && catkin build hrpsys --force-cmake --no-status --no-notify" && \
    catkin clean -b -l -y
