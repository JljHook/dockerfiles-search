# Source
FROM gentoo/stage3-amd64:latest

# Sync portage
RUN mkdir -p /usr/portage/distfiles /usr/portage/metadata /usr/portage/packages && \
chown -R portage:portage /usr/portage && \
echo "masters = gentoo" > /usr/portage/metadata/layout.conf && \
emerge-webrsync && \
# Enable systemd
eselect profile set "default/linux/amd64/17.0/systemd" && \
# Uninstall udev and sysvinit
emerge --unmerge sys-apps/sysvinit sys-fs/eudev virtual/dev-manager virtual/udev && \
# Install base packages
emerge app-portage/gentoolkit && \
# 2018-09-04 Rebuild curl to fix cmake build
emerge net-misc/curl && \
# Install systemd
emerge sys-apps/systemd && \
# Setup systemd
systemd-machine-id-setup && \
# Strip down systemd
rm -f /etc/systemd/system/*.wants/* && \
# Cleanup portage, downloaded source files and compile directories
rm -rf /usr/portage /var/tmp/* && \
# Purge logs
rm -rf /var/log/* /var/run/log/journal/*

# Without this, init won't start the enabled services. Starting the services
# fails with one of:
#     Failed to get D-Bus connection: Operation not permitted
#     System has not been booted with systemd as init system (PID 1). Can't operate.
#     Failed to connect to bus: No such file or directory
VOLUME [ "/sys/fs/cgroup", "/run" ]

# Start via systemd
CMD ["/usr/lib64/systemd/systemd"]

# Image metadata
LABEL org.label-schema.vendor="Mesaguy" \
      org.label-schema.name="Gentoo Linux latest rolling-release boot" \
      org.label-schema.description="Image boots Gentoo Linux's latest rolling-release via systemd" \
      com.mesaguy.operatingsystem.eol="false"
