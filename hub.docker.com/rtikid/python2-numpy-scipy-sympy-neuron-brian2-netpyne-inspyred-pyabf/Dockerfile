#FROM opensciencegrid/osgvo-ubuntu-xenial
FROM ubuntu:18.10

#enable OSG cvmfs 
RUN mkdir /cvmfs
# update and install required packages. Some of then may be substituted by anaconda.
RUN apt update && apt upgrade -y --no-install-recommends
RUN apt install -y --no-install-recommends apt-utils
RUN apt install -y --no-install-recommends build-essential g++ gfortran \
	sudo git gsl-bin libgsl-dev file \
	libzzip-dev libzlcore-data libzlcore-dev h5utils libhdf5-dev \
	libblas-dev liblapack-dev \
    ncurses-dev libreadline-dev openmpi-bin openmpi-common libopenmpi-dev \
    libssl-dev openssl libffi-dev wget 

RUN mkdir /usr/local/x86_64 && cd /usr/local/x86_64 && ln -s ../bin && ln -s ../lib && ln -s ../lib64 && ln -s ../sbin && ln -s ../share && \
    [ -d "/usr/src" ]||{ mkdir /usr/src; } 
WORKDIR /usr/src
    
# Install python 
RUN wget --no-check-certificate https://www.python.org/ftp/python/2.7.16/Python-2.7.16.tgz
RUN tar -xvf Python-2.7.16.tgz && cd Python-2.7.16 && \
	./configure --prefix=/usr --enable-shared --enable-ipv6 --without-tcltk --enable-optimizations && \
	make && make install && cd ../ && rm -fR Python-2.7.16 Python-2.7.16.tgz

# Install PIP
RUN wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py 
RUN python get-pip.py && rm get-pip.py

# #### Install required packeges ####
RUN pip install numpy && \
	pip install scipy && \
	pip install sympy && \
	pip install argparse PyHamcrest && \
    pip install brian2 && \
    pip install --upgrade pyabf && \
    pip install netpyne && \
    pip install inspyred
    

# ### Installing NEURON ###
RUN wget --no-check-certificate https://neuron.yale.edu/ftp/neuron/versions/v7.6/7.6.7/nrn-7.6.7.tar.gz   &&\
    tar -xvf nrn-7.6.7.tar.gz
#BUILD Neuron
RUN cd nrn-7.6 && ./configure --prefix=/usr/local --without-x --with-nrniv --with-nrnpython=dynamic --with-pyexe=/usr/bin/python2 --with-mpi --with-paranrn && make && make install && \
    cd /usr/local/lib/python && cp -a * /usr/lib/python2.7/site-packages && \
    cd /usr/local/lib/ && rm -fR python && ln -s /usr/lib/python2.7/site-packages python && \
    cd /usr/src && rm -fR nrn-7.6.7.tar.gz

#RTH module
COPY mrth2.py /usr/lib/python2.7/site-packages/mrth.py

RUN python -c "import mrth; mrth.versions()"
WORKDIR /home
RUN cd /home
