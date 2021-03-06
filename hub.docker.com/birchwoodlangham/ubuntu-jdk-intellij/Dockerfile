FROM birchwoodlangham/ubuntu-jdk:latest

LABEL maintainer="tan.quach@birchwoodlangham.com"

ENV DEBIAN_FRONTEND noninteractive

# install zsh, python pip etc.
RUN apt-get update && \
    apt-get install -y -qq --fix-missing python-pip python-dev powerline exuberant-ctags \
    software-properties-common git libxext-dev libxrender-dev libxslt1.1 \
    libxtst-dev libgtk2.0-0 libcanberra-gtk-module libxss1 libxkbfile1 \
    gconf2 gconf-service libnotify4 libnss3 gvfs-bin xdg-utils && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    pip install psutil thefuck sexpdata websocket-client && \
    mkdir -p /opt/idea && \
    wget https://download.jetbrains.com/idea/ideaIU-2019.1.3-no-jbr.tar.gz && \
    tar -C /opt/idea -zxf ideaIU-2019.1.3-no-jbr.tar.gz --strip-components=1 && \
    rm ideaIU-2019.1.3-no-jbr.tar.gz && \
    useradd -d /home/user -m -U user

USER user
WORKDIR /home/user

# Use this one to install the plugins etc.
COPY vimrc_plugins /home/user/.vimrc

# Now for vim plugins, the powerline fonts and nerd fonts required for powerline
RUN git clone https://github.com/powerline/fonts.git && \
    fonts/install.sh && \
    rm -rf fonts && \
    mkdir -p /home/user/.vim && \
    git clone https://github.com/VundleVim/Vundle.vim.git /home/user/.vim/bundle/Vundle.vim && \
    vim +PluginInstall +qall 
# copy configuration files for vim, zsh and tmux
COPY vimrc /home/user/.vimrc

VOLUME ["/home/user"]

ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

CMD ["/opt/idea/bin/idea.sh"]
