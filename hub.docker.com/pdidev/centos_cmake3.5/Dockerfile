FROM pdidev/centos_base

USER 0

RUN yum install -y --setopt=tsflags=nodocs \
    epel-release \
 && yum install -y --setopt=tsflags=nodocs \
    hdf5-openmpi3-devel \
 && yum clean all -y

USER 1001

RUN wget https://cmake.org/files/v3.5/cmake-3.5.2.tar.gz \
 && tar -xzf cmake-3.5.2.tar.gz
RUN cd cmake-3.5.2 \
 && ./configure \
 && make -j 2

USER 0

RUN cd /opt/app-root/src/cmake-3.5.2 \
 && make install

USER 1001

RUN rm -rf cmake*
