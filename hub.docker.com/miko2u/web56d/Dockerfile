FROM miko2u/web56

# Create dropbox service account and set permissions.
RUN curl -o /usr/bin/dropboxctl -sSL https://www.dropbox.com/download?dl=packages/dropbox.py \
 && chmod 755 /usr/bin/dropboxctl \
 && mkdir -p /var/www \
 && groupadd -g 500 staff \
 && useradd -u 500 -g 500 -m -d /var/www/virtual staff \
 && chmod 711 /var/www/virtual \
 && echo -e "staff\tALL=(ALL)\tNOPASSWD:ALL" > /etc/sudoers.d/staff \
 && groupadd -o -g 500 dropbox \
 && useradd -o -u 500 -g dropbox -c "Dropbox user" -s /sbin/nologin dropbox \
 && chmod 755 /home/dropbox \
 && gosu dropbox mkdir -p /home/dropbox/.dropbox /home/dropbox/.dropbox-dist /home/dropbox/Dropbox \
 && gosu dropbox /bin/bash -c 'echo y | /usr/bin/dropboxctl start -i' \
 && gosu dropbox /usr/bin/dropboxctl stop \
 && rm -rf /home/dropbox/.dropbox/* \
 && chmod 755 /home/dropbox/Dropbox \
 && printf '#!/bin/sh\n\
gosu dropbox /usr/bin/dropboxctl $@\n\
' > /usr/bin/dropbox \
 && chmod 755 /usr/bin/dropbox

# init
COPY prepare /boot/
