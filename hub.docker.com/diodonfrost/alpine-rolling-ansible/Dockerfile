FROM alpine:latest
LABEL maintainer="diodonfrost <diodon.frost@diodonfrost.me>"

# Install Ansible
RUN apk add --no-cache git ansible sudo openssh unzip

VOLUME ["/sys/fs/cgroup"]

CMD ["/sbin/init"]
