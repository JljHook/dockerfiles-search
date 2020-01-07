FROM centos:centos7

## Set a default user. Available via runtime flag `--user docker` 
## Add user to 'staff' group, granting them write privileges to /usr/local/lib/R/site.library
## User should also have & own a home directory (for rstudio or linked volumes to work properly). 
RUN useradd docker \
	&& mkdir -p /home/docker \
	&& chown docker:docker /home/docker
 
RUN yum -y update; yum clean all
RUN yum -y install epel-release; yum clean all

# install packages for authentication with Active Directory
RUN yum -y install authconfig krb5-workstation pam_krb5 samba-common oddjob-mkhomedir sudo ntp; yum clean all

# create /opt/install folder
# RUN mkdir -p /opt/install/

# RUN ls -ld /usr/lib64/R/library

RUN mkdir -p /usr/lib64/R/library \
    && chown docker:docker /usr/lib64/R/library \
    && mkdir -p /usr/share/doc/R-3.3.3/html \
    && chown docker:docker /usr/lib64/R/library 

RUN yum -y install R; yum clean all

# Install MSSQL driver
RUN curl -o /etc/yum.repos.d/mssql-release.repo https://packages.microsoft.com/config/rhel/7/prod.repo && echo "curled" \
    && yum remove unixODBC-utf16 unixODBC-utf16-devel \
    && ACCEPT_EULA=Y yum install -y msodbcsql-13.1.4.0-1 mssql-tools-14.0.3.0-1 unixODBC-devel && echo "installed" \
    && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile && echo "exported to bash_profile" \
    && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc && echo "exported to bashrc" \
    && source ~/.bashrc

# Setup R configs. R package versions are not yet specified; will use most recent.
RUN echo "r <- getOption('repos'); r['CRAN'] <- 'http://cran.us.r-project.org'; options(repos = r); .libPaths('/usr/lib64/R/library')" > ~/.Rprofile \
    && Rscript -e "install.packages(c('ggplot2', 'needs', 'jsonlite', 'dplyr', 'RODBC', 'healthcareai'))"
# install any other packages here

# CentOS 7 does not have bzip2 and miniconda requires it for installation
RUN yum -y install bzip2; yum clean all

RUN curl -N https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh --output usr/src/Miniconda.sh \
   && bash usr/src/Miniconda.sh -b -p /opt/conda \
   && source /opt/conda/bin/activate \
   && /opt/conda/bin/conda install -y numpy \
   && /opt/conda/bin/conda list

ENV PATH /opt/conda/bin:$PATH

# RUN curl -sSL  | bash

#install node.js v6
# RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.25.0/install.sh | bash

# Install nvm with node and npm
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 6.10.1

# Install nvm with node and npm
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash \
    && source $NVM_DIR/nvm.sh \
    && nvm --version \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH      $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN node --version

RUN rm -rf /var/cache/yum
