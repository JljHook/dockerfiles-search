FROM janzi/vsts-build-agents:ubuntu-16.04

# Install basic command-line utilities
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    curl \
    dnsutils \
    file \
    ftp \
    iproute2 \
    iputils-ping \
    locales \
    openssh-client \
    rsync\
    shellcheck \
    sudo \
    telnet \
    time \
    unzip \
    wget \
    zip \
    tzdata \
 && rm -rf /var/lib/apt/lists/*

# Setup the locale
ENV LANG en_US.UTF-8
ENV LC_ALL $LANG
RUN locale-gen $LANG \
 && update-locale

# Accept EULA - needed for certain Microsoft packages like SQL Server Client Tools
ENV ACCEPT_EULA=Y

# Install essential build tools
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    build-essential \
 && rm -rf /var/lib/apt/lists/*

# Install Ansible
RUN apt-get update \   
 && apt-get install -y --no-install-recommends \
    ansible \      
 && rm -rf /var/lib/apt/lists/*

# Install Azure CLI (instructions taken from https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
RUN echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $(lsb_release -cs) main" \
  | tee /etc/apt/sources.list.d/azure-cli.list \
 && curl -L https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
    apt-transport-https \
    azure-cli \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/* \
 && az --version

# Install CMake
RUN curl -sL https://cmake.org/files/v3.10/cmake-3.10.2-Linux-x86_64.sh -o cmake.sh \
 && chmod +x cmake.sh \
 && ./cmake.sh --prefix=/usr/local --exclude-subdir \
 && rm cmake.sh

# Install Helm
RUN curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash

# Install Heroku CLI
RUN curl https://cli-assets.heroku.com/install.sh | sh

# Install Java OpenJDKs
RUN apt-add-repository -y ppa:openjdk-r/ppa
RUN apt-get update \
 && apt-get install -y --no-install-recommends openjdk-8-jdk \
 && rm -rf /var/lib/apt/lists/*
RUN apt-get update \
 && apt-get install -y --no-install-recommends openjdk-11-jdk \
 && rm -rf /var/lib/apt/lists/*
RUN update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
ENV JAVA_HOME_8_X64=/usr/lib/jvm/java-8-openjdk-amd64 \
    JAVA_HOME_11_X64=/usr/lib/jvm/java-11-openjdk-amd64 \
    JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
    JAVA_TOOL_OPTIONS=-Dfile.encoding=UTF8

# Install Java Tools (Ant, Gradle, Maven)
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    ant \
    ant-optional \
 && rm -rf /var/lib/apt/lists/*
RUN curl -sL https://services.gradle.org/distributions/gradle-4.6-bin.zip -o gradle-4.6.zip \
 && unzip -d /usr/share gradle-4.6.zip \
 && ln -s /usr/share/gradle-4.6/bin/gradle /usr/bin/gradle \
 && rm gradle-4.6.zip
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    maven \
 && rm -rf /var/lib/apt/lists/*
ENV ANT_HOME=/usr/share/ant \
    GRADLE_HOME=/usr/share/gradle \
    M2_HOME=/usr/share/maven

# Install kubectl
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
 && chmod +x ./kubectl \
 && mv ./kubectl /usr/local/bin/kubectl

# Install Miniconda
RUN curl -sL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o miniconda.sh \
 && chmod +x miniconda.sh \
 && ./miniconda.sh -b -p /usr/share/miniconda \
 && rm miniconda.sh
ENV CONDA=/usr/share/miniconda

# Install Mono
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF \
 && echo "deb https://download.mono-project.com/repo/ubuntu stable-xenial main" | tee /etc/apt/sources.list.d/mono-official-stable.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends apt-transport-https mono-complete \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/*

# Install MS SQL Server client tools (https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-tools?view=sql-server-2017)
RUN [ "xenial" = "xenial" ] \
  && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
  && curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | tee /etc/apt/sources.list.d/msprod.list \
  && apt-get update \
  && apt-get install -y mssql-tools unixodbc-dev \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /etc/apt/sources.list.d/* \
  || echo -n
ENV PATH=$PATH:/opt/mssql-tools/bin

# Install MySQL Client
RUN apt-get update \
  && apt-get install -y mysql-client \
  && rm -rf /var/lib/apt/lists/*
ENV mysql=/usr/bin/mysql

# Install MySQL Server
ENV MYSQL_ROOT_PASSWORD=
RUN bash -c 'debconf-set-selections <<< "mysql-server mysql-server/root_password password $MYSQL_ROOT_PASSWORD"'
RUN bash -c 'debconf-set-selections <<< "mysql-server mysql-server/root_password_again password $MYSQL_ROOT_PASSWORD"'
RUN apt-get update \
  && apt-get install -y mysql-server \
  && rm -rf /var/lib/apt/lists/*

# Install .NET Core SDK and initialize package cache
RUN curl https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb > packages-microsoft-prod.deb \
 && dpkg -i packages-microsoft-prod.deb \
 && rm packages-microsoft-prod.deb \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
    apt-transport-https \
    dotnet-sdk-2.2 \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/*
RUN dotnet help
ENV dotnet=/usr/bin/dotnet
ENV DOTNET_CLI_TELEMETRY_OPTOUT=1

# Install AzCopy (depends on .NET Core)
RUN apt-key adv --keyserver packages.microsoft.com --recv-keys EB3E94ADBE1229CF \
 && echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod/ xenial main" | tee /etc/apt/sources.list.d/azure.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends azcopy \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/*

# Install LTS Node.js and related tools
RUN curl -sL https://git.io/n-install | bash -s -- -ny - \
 && ~/n/bin/n lts \
 && npm install -g bower \
 && npm install -g grunt \
 && npm install -g gulp \
 && npm install -g n \
 && npm install -g webpack webpack-cli --save-dev \
 && npm install -g parcel-bundler \
 && npm i -g npm \
 && rm -rf ~/n
ENV bower=/usr/local/bin/bower \
    grunt=/usr/local/bin/grunt

# # Install PhantomJS
# RUN apt-get update \
#  && apt-get install -y chrpath libssl-dev libxft-dev libfreetype6 libfreetype6-dev libfontconfig1 libfontconfig1-dev \
#  && rm -rf /var/lib/apt/lists/* \
#  && export PHANTOM_JS=phantomjs-2.1.1-linux-x86_64 \
#  && wget https://bitbucket.org/ariya/phantomjs/downloads/$PHANTOM_JS.tar.bz2 \
#  && tar xvjf $PHANTOM_JS.tar.bz2 \
#  && mv $PHANTOM_JS /usr/local/share \
#  && ln -sf /usr/local/share/$PHANTOM_JS/bin/phantomjs /usr/local/bin

# # Install Pollinate
# RUN apt-get update \
#  && apt-get install -y --no-install-recommends pollinate \
#  && rm -rf /var/lib/apt/lists/*

# Install Powershell Core
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
 && curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | tee /etc/apt/sources.list.d/microsoft.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
    powershell \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/*

# Instally PyPy2
RUN wget -q -P /tmp https://bitbucket.org/pypy/pypy/downloads/pypy2-v6.0.0-linux64.tar.bz2 \
 && tar -x -C /opt -f /tmp/pypy2-v6.0.0-linux64.tar.bz2 \
 && rm /tmp/pypy2-v6.0.0-linux64.tar.bz2 \
 && mv /opt/pypy2-v6.0.0-linux64 /opt/pypy2 \
 && ln -s /opt/pypy2/bin/pypy /usr/local/bin/pypy

# Install PyPy3
RUN wget -q -P /tmp https://bitbucket.org/pypy/pypy/downloads/pypy3-v6.0.0-linux64.tar.bz2 \
 && tar -x -C /opt -f /tmp/pypy3-v6.0.0-linux64.tar.bz2 \
 && rm /tmp/pypy3-v6.0.0-linux64.tar.bz2 \
 && mv /opt/pypy3-v6.0.0-linux64 /opt/pypy3 \
 && ln -s /opt/pypy3/bin/pypy3 /usr/local/bin/pypy3

# Install Python
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    python \
    python-pip \
    python3 \
    python3-pip \
 && rm -rf /var/lib/apt/lists/*

# Install Scala build tools
RUN curl -s https://raw.githubusercontent.com/paulp/sbt-extras/master/sbt > /usr/local/bin/sbt \
 && chmod 0755 /usr/local/bin/sbt

# Install Terraform
RUN TERRAFORM_VERSION=$(curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r .current_version) \
 && curl -LO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
 && unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /usr/local/bin \
 && rm -f terraform_${TERRAFORM_VERSION}_linux_amd64.zip

# XSLT transformation
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    xsltproc \
    xalan \
 && rm -rf /var/lib/apt/lists/*

# Install yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
 && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends yarn \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/*

# Install Xvfb
RUN apt-get update \
 && apt-get install -y xvfb \
 && rm -rf /var/lib/apt/lists/*

# Download hosted tool cache
ENV AGENT_TOOLSDIRECTORY=/opt/hostedtoolcache
RUN azcopy --recursive --source https://vstsagenttools.blob.core.windows.net/tools/hostedtoolcache/linux --destination $AGENT_TOOLSDIRECTORY

# Install the tools from the hosted tool cache
RUN original_directory=$PWD \
 && setups=$(find $AGENT_TOOLSDIRECTORY -name setup.sh) \
 && for setup in $setups; do \
        chmod +x $setup; \
        cd $(dirname $setup); \
        ./$(basename $setup); \
        cd $original_directory; \
    done;

# Clean system
RUN apt-get clean \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/*
