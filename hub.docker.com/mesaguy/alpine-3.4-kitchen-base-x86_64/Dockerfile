FROM mesaguy/alpine-3.4-boot-x86_64:latest

# Install bash, sudo, and openssh for kitchen
RUN apk --no-cache add bash openssh sudo && \
# Purge logs
rm -rf /var/log/*

# Adjust sshd defaults per kitchen's needs
RUN test -f /usr/lib/ssh/sftp-server && \
printf "Subsystem sftp /usr/lib/ssh/sftp-server\\nUseDNS no\\nUsePAM no\\nPasswordAuthentication yes\\nUsePrivilegeSeparation no\\n" > /etc/ssh/sshd_config && \
# Start SSH on boot
rc-update add sshd && \
# Add 'kitchen' user
if ! getent passwd kitchen; then adduser -h /home/kitchen -s /bin/bash -D kitchen; fi && \
# Ensure 'kitchen' user account is not locked out
sed -i 's/kitchen:!/kitchen:/g' /etc/shadow && \
# Allow unlimited sudo privileges to 'kitchen' user
echo "kitchen ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && echo "Defaults !requiretty" >> /etc/sudoers && \
# Make 'kitchen' user's ~/.ssh directory
mkdir -m 0700 /home/kitchen/.ssh && \
# Make 'kitchen' user's authorized_keys file (empty)
touch /home/kitchen/.ssh/authorized_keys && \
# Secure 'kitchen' user's authorized_keys file
chmod 0600 /home/kitchen/.ssh/authorized_keys && \
# Fix 'kitchen' user's SSH directory permissions
chown -R kitchen:kitchen /home/kitchen/.ssh

# Expose SSH
EXPOSE 22

# Image metadata
LABEL org.label-schema.vendor="Mesaguy" \
      org.label-schema.name="Alpine Linux 3.4 kitchen-base" \
      org.label-schema.description="Image boots Alpine Linux 3.4 via OpenRC configured for kitchen testing" \
      com.mesaguy.operatingsystem.eol="true"
