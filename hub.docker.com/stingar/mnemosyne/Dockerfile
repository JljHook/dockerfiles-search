FROM centos:centos7

LABEL maintainer Alexander Merck <alexander.t.merck@gmail.com>
LABEL name "chn-mnemosyne"
LABEL version "0.1"
LABEL release "1"
LABEL summary "Community Honey Network mnemosyne server"
LABEL description "mnemosyne is a normalizer for honeypot data."
LABEL authoritative-source-url "https://github.com/CommunityHoneyNetwork/mnemosyne/commits/master"

ENV playbook "mnemosyne.yml"

RUN yum install -y epel-release \
    && yum install -y ansible

RUN echo "localhost ansible_connection=local" >> /etc/ansible/hosts
ADD . /opt/
RUN ansible-playbook /opt/${playbook}

EXPOSE 8181

ENTRYPOINT ["/sbin/runsvdir", "-P", "/etc/service"]