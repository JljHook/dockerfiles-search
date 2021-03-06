FROM debian:latest
MAINTAINER Yanis Pavlidis <yanis@pavlidis.email>

RUN apt-get -qq update && apt-get -qq -y install curl bzip2 \
    && curl -sSL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o /tmp/miniconda.sh \
    && bash /tmp/miniconda.sh -bfp /usr/local \
    && rm -rf /tmp/miniconda.sh \
    && conda install -y python=3 \
    && conda update conda \
    && apt-get -qq -y remove curl bzip2 \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes
# end of continuumio/miniconda3

# install boost
RUN apt-get update                  && \
    apt-get install -y curl gcc g++ && \
    mkdir /boost                    && \
    cd /boost                       && \
    curl -L https://dl.bintray.com/boostorg/release/1.70.0/source/boost_1_70_0.tar.gz --output boost_1_70_0.tar.gz && \
    tar -xzf boost_1_70_0.tar.gz    && \
    cd /boost/boost_1_70_0          && \
    ./bootstrap.sh                  && \
    ./b2 install -d0 --without-wave --without-python && \
    cd /                            && \
    rm -rf boost                    && \
    apt-get -qq -y remove curl      && \
    apt-get -qq -y autoremove       && \
    apt-get autoclean               && \
    rm -rf /var/lib/apt/lists/* /var/log/dpkg.log

RUN conda install cling -c QuantStack -c conda-forge -y      && \
    conda install xeus-cling -c QuantStack -c conda-forge -y && \
    conda install jupyterlab -c conda-forge -y                 && \
    conda install nodejs -y &&\
    conda clean --all --yes

# set up user and environment
ENV LANG en-US
ENV PATH /opt/conda/bin:$PATH
WORKDIR /usr/src/notebooks
CMD jupyter lab --ip='*' --port=8888 --no-browser --allow-root
