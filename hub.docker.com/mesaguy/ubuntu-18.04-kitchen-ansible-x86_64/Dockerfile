FROM mesaguy/ubuntu-18.04-kitchen-base-x86_64:latest

# Update repo definitions
RUN apt update && \
# Install python for ansible
apt install -y python && \
# Clean (purge) the repo cache
apt-get clean && \
rm -rf /var/lib/apt/lists/* && \
# Purge logs
rm -rf /var/log/*

# Image metadata
LABEL org.label-schema.vendor="Mesaguy" \
      org.label-schema.name="Ubuntu Linux 18.04 (Bionic Beaver) kitchen-ansible" \
      org.label-schema.description="Image boots Ubuntu Linux 18.04 (Bionic Beaver) via systemd configured for kitchen ansible testing" \
      com.mesaguy.operatingsystem.eol="false"
