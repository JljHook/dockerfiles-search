# Dockerfile for building Ansible image for Debian 9 (stretch), with as few additional software as possible.
#
# @see https://launchpad.net/~ansible/+archive/ubuntu/ansible
#
#


FROM debian:stretch

MAINTAINER Alioua Larbi <alioua.larbi@gmail.com>


RUN echo "===> Installing python, sudo, and other supporting tools..."  && \
    apt-get update -y  &&  apt-get install --fix-missing          && \
    DEBIAN_FRONTEND=noninteractive         \
    apt-get install -y                     \
        python python-yaml sudo            \
        curl wget git gcc python-pip python-dev libffi-dev libssl-dev  && \
    apt-get -y --purge remove python-cffi          && \
    pip install --upgrade pycrypto cffi pywinrm    && \
    \
    \
    \
    echo "===> Installing Ansible..."   && \
    pip install ansible                 && \
    \
    \
    \
    echo "===> Installing handy tools (not absolutely required)..."  && \
    apt-get install -y sshpass openssh-client  && \
    \
    \
    echo "===> Removing unused APT resources..."                  && \
    apt-get -f -y --auto-remove remove \
                 gcc python-pip python-dev libffi-dev libssl-dev  && \
    apt-get clean                                                 && \
    rm -rf /var/lib/apt/lists/*  /tmp/*                           && \
    \
    \
    echo "===> Adding hosts for convenience..."        && \
    mkdir -p /etc/ansible                              && \
    echo 'localhost' > /etc/ansible/hosts


# default command: display Ansible version
CMD [ "ansible-playbook", "--version" ]
