FROM ubuntu:xenial
MAINTAINER Pavel Sviderski <ps@stepic.org>
#MAINTAINER Iakov Kirilenko <Iakov.Kirilenko@trikset.com>
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update
RUN apt-get -y dist-upgrade

RUN useradd -M -d /sandbox sandbox

RUN apt-get install -y locales apt-utils gnupg2

RUN  apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1E9377A2BA9EF27F \
   &&  apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 10C56D0DE9977759


#RUN apt install software-properties-common -y
#RUN add-apt-repository -y ppa:beineri/opt-qt563-trusty
#RUN echo "deb http://ppa.launchpad.net/ubuntu-toolchain-r/test/ubuntu xenial main" > \
#    /etc/apt/sources.list.d/toolchain-r-test.list \

RUN echo "deb http://ppa.launchpad.net/beineri/opt-qt563-xenial/ubuntu xenial main" > \
    /etc/apt/sources.list.d/opt-qt-xenial.list

RUN  apt-get update
RUN  apt-get install -y --no-install-recommends \
    tar bzip2 xz-utils \
    libxext6 libgl1-mesa-glx libstdc++6 unzip libfreetype6 fontconfig libxrender1 \
    qt56base qt56svg qt56script qt56multimedia \
    libpython3.5

RUN apt-get install -y libubsan0

RUN apt-get install -y git

RUN apt-get clean all
RUN locale-gen ru_RU.UTF-8 \
  && /bin/echo -e "/opt/qt56/lib/x86_64-linux-gnu\n/opt/qt56/lib\n" > /etc/ld.so.conf.d/zz_opt_qt.conf \
  && ldconfig
ENV LANG ru_RU.UTF-8
ADD trik_checker.tar.xz /
RUN chown -R root:root /trikStudio-checker
