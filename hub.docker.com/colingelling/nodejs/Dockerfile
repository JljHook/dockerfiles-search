FROM phusion/baseimage:0.9.22

# Install node 8.x environment
RUN curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
RUN bash nodesource_setup.sh

# Install yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# Install Ubuntu Packages
RUN apt-get update && apt-get install nodejs git-core libpng-dev yarn vim wget htop -y

# Setup HOME Environment variable
ENV HOME /root

CMD ["/sbin/my_init"]

# Set terminal to non-interactive
ENV DEBIAN_FRONTEND=noninteractive

# Create new symlink to UTC timezone for localtime
RUN unlink /etc/localtime
RUN ln -s /usr/share/zoneinfo/UTC /etc/localtime

# Copy friendly bashrc file for docker identification in terminal
COPY .build/.bashrc /root/.bashrc

# Copy startup script script for node services
COPY .build/01_node_services.sh /etc/my_init.d/01_node_services.sh
RUN chmod +x /etc/my_init.d/01_node_services.sh

# Set terminal environment
ENV TERM=xterm

# Setup working dir/create empty public folder
WORKDIR /home/app
RUN mkdir -p /home/app/public

# Setup public folder permissions
RUN chmod -R 755 /home/app/public

# Get and install the Yarn package
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt update && apt install -y yarn

# Add sass-loader with node-sass packages
RUN yarn add -D sass-loader node-sass

# Install Vue
RUN yarn global add @vue/cli

# Clean up APT when done
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*