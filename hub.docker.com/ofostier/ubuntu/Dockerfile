FROM ubuntu:trusty
MAINTAINER Olivier FOSTIER <ofostier@gmail.com>

# update
RUN apt-get -y update

# set UTF-8 locale
RUN locale-gen en_US.UTF-8 && \
    echo 'LANG="en_US.UTF-8"' > /etc/default/locale

CMD ["/bin/bash"]
