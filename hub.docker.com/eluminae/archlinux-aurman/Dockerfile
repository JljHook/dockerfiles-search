FROM base/devel

# Use `/usr/bin/true` as a dummy editor
ENV EDITOR=true

RUN echo '[multilib]' >> /etc/pacman.conf && \
    echo 'Include = /etc/pacman.d/mirrorlist' >> /etc/pacman.conf && \
    pacman --noconfirm -Syyu && \
    pacman --noconfirm -S git && \
    useradd -m -s /bin/bash aur && \
    passwd -d aur && \
    echo 'aur ALL=(ALL) ALL' > /etc/sudoers.d/aur && \
    echo 'Defaults env_keep += "EDITOR"' >> /etc/sudoers.d/aur && \
    mkdir -p /home/aur/.gnupg && \
    echo 'standard-resolver' > /home/aur/.gnupg/dirmngr.conf && \
    chown -R aur:aur /home/aur && \
    mkdir /build && \
    chown -R aur:aur /build && \
    cd /build && \
    sudo -u aur gpg --recv-key 465022E743D71E39 && \
    sudo -u aur git clone --depth 1 https://github.com/polygamma/aurman.git && \
    cd aurman && \
    sudo -u aur makepkg --noconfirm -si && \
    yes | pacman -Scc && \
    cd / && \
    rm -rf /build

CMD ["/bin/bash"]
