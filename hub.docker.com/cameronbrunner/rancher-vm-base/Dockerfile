FROM ubuntu:14.04.2
RUN apt-get update && apt-get install -y qemu-kvm dnsmasq bridge-utils
COPY startvm /var/lib/rancher/startvm
ENTRYPOINT ["/var/lib/rancher/startvm"]
VOLUME /image
CMD []
