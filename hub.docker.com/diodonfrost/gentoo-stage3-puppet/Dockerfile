FROM gentoo/stage3-amd64:latest
MAINTAINER diodonfrost <diodon.frost@diodonfrost.me>

# Install systemd
RUN emerge-webrsync && \
    emerge --quiet --unmerge sys-fs/eudev && \
    mkdir /etc/portage/package.mask && \
    echo ">=sys-apps/dbus-1.10.12 systemd" > /etc/portage/package.use/iputils && \
    echo "sys-libs/pam" > /etc/portage/package.mask/pam && \
    emerge --quiet --pretend --tree sys-apps/systemd && \
    emerge sys-apps/systemd && \
    emerge --quiet app-portage/eix && \
    touch /var/cache/eix/portage.eix && chown portage:portage /var/cache/eix/portage.eix && eix-update && \
    rm -rf /usr/portage/*

# Install puppet
RUN emerge-webrsync && \
    emerge --quiet net-misc/openssh app-admin/sudo app-admin/puppet && \
    rm -rf /usr/portage/*

CMD ["/usr/lib/systemd/systemd"]
