FROM centos:7

LABEL org.opencontainers.image.title="haxorof/ansible" \
    org.opencontainers.image.description="Ansible + additions" \
    org.opencontainers.image.source="https://github.com/haxorof/docker-ansible" \
    org.opencontainers.image.licenses="MIT"    

ENV DOCKER_VERSION 19.03.1

COPY requirements.txt .

RUN yum install -y epel-release \
    && yum upgrade -y \
    && yum install -y \
    openssh \
    sshpass \
    git \
    python-pip \
    wget \
    curl \
    && python -m pip install --no-cache-dir --upgrade pip \
    && python -m pip install --no-cache-dir -r requirements.txt \
    && wget -O - https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}.tgz | tar -xz -C /usr/lib \
    && ln -s /usr/lib/docker/docker /usr/bin/docker \
    && mkdir -p /etc/ansible/roles \
    && echo 'localhost ansible_connection=local ansible_python_interpreter=/usr/bin/python' > /etc/ansible/hosts \
    && yum clean all \
    && rm -rf /var/cache/yum \
    && useradd -u 1000 ansible-1000 \
    && useradd -u 1001 ansible-1001

WORKDIR /mnt

CMD [ "ansible-playbook", "playbook.yml" ]
