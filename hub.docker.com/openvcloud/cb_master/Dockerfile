# Before starting please add a private key which has access to the priv repos in id_rsa
FROM openvcloud/js7

COPY system /opt/jumpscale7/hrd/system/
COPY apps /opt/jumpscale7/hrd/apps/
COPY atyourservice.hrd /opt/jumpscale7/hrd/system/
COPY whoami.hrd /opt/jumpscale7/hrd/system/

COPY start.sh /
COPY services-migrator.py /tmp/
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# add configuration script
RUN mkdir -p /var/run/scripts/system /opt/cfg/system /opt/jumpscale7/var/resourcetracking/ /opt/cfg/version
COPY configure-system.py /var/run/scripts/system
RUN mkdir -p /var/run/scripts/portal/
COPY configure-portal.py /var/run/scripts/portal/

RUN touch /root/.ssh/known_hosts && \
    ssh-keyscan github.com >> /root/.ssh/known_hosts && \
    ssh-keyscan git.aydo.com >> /root/.ssh/known_hosts && \
    ssh-keyscan -p 10022 docs.greenitglobe.com >> /root/.ssh/known_hosts

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y graphviz

RUN chmod +x start.sh

RUN ["/bin/bash", "-c","./start.sh"]
