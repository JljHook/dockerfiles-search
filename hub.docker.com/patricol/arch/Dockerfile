FROM archlinux/base

RUN pacman --noconfirm --needed -Syu \
    base-devel git vim pacman-contrib \
 && groupadd -r sudo \
 && useradd -u 99 -m -g users -G sudo -s /bin/bash user \
 && echo '%sudo  ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
 && echo 'echo "Sorting mirrors...."' >> /usr/local/bin/sort-mirrors \
 && echo 'curl -s "https://www.archlinux.org/mirrorlist/?country=US&protocol=https&use_mirror_status=on" | sed -e "s/^#//" -e "/^#/d" | rankmirrors -n 50 - | sudo tee /etc/pacman.d/mirrorlist' >> /usr/local/bin/sort-mirrors \
 && echo 'yay --noconfirm --needed --removemake --cleanafter -Syu $@' >> /usr/local/bin/addpackages \
 && chmod +x /usr/local/bin/* \
 && sed -i '/^#en_US.UTF-8 UTF-8/s/^#//' /etc/locale.gen \
 && locale-gen \
 && echo 'LANG=en_US.UTF-8' | tee -a /etc/locale.conf \
 && unset LANG \
 && source /etc/profile.d/locale.sh \
 && (yes | pacman -Scc || true)

USER user

WORKDIR /home/user

RUN unset LANG \
 && source /etc/profile.d/locale.sh \
 && git clone https://aur.archlinux.org/yay.git \
 && cd yay \
 && makepkg -sci --noconfirm \
 && cd .. \
 && rm -rf ./yay/ \
 && yay --noconfirm -c \
 && (yes | yay -Scc || true)


CMD tail -f /dev/null
