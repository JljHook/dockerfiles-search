# Source
FROM debian:9

# Update repo definitions
RUN apt update && \
# Upgrade all packages
apt dist-upgrade -y && \
# Install systemd
apt install -y systemd && \
# Don't start any optional services except for the few we need.
find /etc/systemd/system \
/lib/systemd/system \
-path '*.wants/*' \
-not -name '*journald*' \
-not -name '*systemd-tmpfiles*' \
-not -name '*systemd-user-sessions*' \
-exec rm \{\} \; && \
# Clean (purge) the repo cache
apt-get clean && \
rm -rf /var/lib/apt/lists/* && \
# Purge logs
rm -rf /var/log/* /var/run/log/journal

# Without this, init won't start the enabled services. Starting the services
# fails with one of:
#     Failed to get D-Bus connection: Operation not permitted
#     System has not been booted with systemd as init system (PID 1). Can't operate.
#     Failed to connect to bus: No such file or directory
VOLUME [ "/sys/fs/cgroup", "/run" ]

# Start via systemd
CMD ["/lib/systemd/systemd"]

# Image metadata
LABEL org.label-schema.vendor="Mesaguy" \
      org.label-schema.name="Debian Linux 9 (Stretch) boot" \
      org.label-schema.description="Image boots Debian Linux 9 (Stretch) via systemd" \
      com.mesaguy.operatingsystem.eol="false"
