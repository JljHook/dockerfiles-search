FROM openjdk:8

LABEL author="Phil Ewels" \
    description="Docker image containing all requirements for NGI-smRNAseq pipeline" \
    maintainer="phil.ewels@scilifelab.se"

# Install container-wide requrements gcc, pip, zlib, libssl, make, libncurses, fortran77, g++, R
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        g++ \
        gawk \
        gcc \
        gfortran \
        libboost-all-dev \
        libbz2-dev \
        libcurl4-openssl-dev \
        libgsl0-dev \
        liblzma-dev \
        libncurses5-dev \
        libpcre3-dev \
        libreadline-dev \
        libssl-dev \
        libtbb2 \
        libgtextutils-dev \
        make \
        python-dev \
        zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# Install pip
RUN curl -fsSL https://bootstrap.pypa.io/get-pip.py -o /opt/get-pip.py && \
    python /opt/get-pip.py && \
    rm /opt/get-pip.py

# Install FASTX-Toolkit
RUN curl -fsSL http://hannonlab.cshl.edu/fastx_toolkit/fastx_toolkit_0.0.13_binaries_Linux_2.6_amd64.tar.bz2  -o /opt/fastx_toolkit_0.0.13_binaries_Linux_2.6_amd64.tar.bz2 && \
    mkdir /opt/fastx_toolkit  && \
    tar -xjf /opt/fastx_toolkit_0.0.13_binaries_Linux_2.6_amd64.tar.bz2 -C /opt/fastx_toolkit && \
    ln -s /opt/fastx_toolkit/bin/*  /usr/local/bin/  && \
    rm /opt/fastx_toolkit_0.0.13_binaries_Linux_2.6_amd64.tar.bz2

# Install FastQC
ENV FASTQC_BIN="fastqc_v0.11.5.zip"
RUN curl -fsSL http://www.bioinformatics.babraham.ac.uk/projects/fastqc/$FASTQC_BIN -o /opt/$FASTQC_BIN && \
    unzip /opt/$FASTQC_BIN -d /opt/ && \
    chmod 755 /opt/FastQC/fastqc && \
    ln -s /opt/FastQC/fastqc /usr/local/bin/fastqc && \
    rm /opt/$FASTQC_BIN

# Install cutadapt
RUN pip install cutadapt

# Install TrimGalore
ENV TRIMGALORE_BIN="trim_galore_v0.4.4.zip"
RUN mkdir /opt/TrimGalore && \
    curl -fsSL http://www.bioinformatics.babraham.ac.uk/projects/trim_galore/$TRIMGALORE_BIN -o /opt/TrimGalore/$TRIMGALORE_BIN && \
    unzip /opt/TrimGalore/$TRIMGALORE_BIN -d /opt/TrimGalore && \
    ln -s /opt/TrimGalore/trim_galore /usr/local/bin/trim_galore && \
    rm /opt/TrimGalore/$TRIMGALORE_BIN

# Install Bowtie
RUN wget -q -O bowtie.zip https://sourceforge.net/projects/bowtie-bio/files/bowtie/1.2.0/bowtie-1.2-linux-x86_64.zip/download && \
  unzip bowtie.zip -d /opt/ && \
  ln -s /opt/bowtie-1.2/bowtie* /usr/local/bin/ && \
  rm bowtie.zip

# Install Bowtie2
RUN wget -q -O bowtie2.zip http://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.3.2/bowtie2-2.3.2-linux-x86_64.zip/download && \
  unzip bowtie2.zip -d /opt/ && \
  ln -s /opt/bowtie2-2.3.2/bowtie2 /usr/local/bin/bowtie2 && \
  rm bowtie2.zip

# Install SAMTools
ENV SAMTOOLS_VERSON="1.4"
RUN curl -fsSL https://github.com/samtools/samtools/releases/download/${SAMTOOLS_VERSON}/samtools-${SAMTOOLS_VERSON}.tar.bz2 -o /opt/samtools-${SAMTOOLS_VERSON}.tar.bz2 && \
    tar xvjf /opt/samtools-${SAMTOOLS_VERSON}.tar.bz2 -C /opt/ && \
    cd /opt/samtools-${SAMTOOLS_VERSON};make;make install && \
    rm /opt/samtools-${SAMTOOLS_VERSON}.tar.bz2

# Install R
ENV R_VERSION="R-3.3.3"
RUN curl -fsSL https://cran.r-project.org/src/base/R-3/${R_VERSION}.tar.gz -o /opt/${R_VERSION}.tar.gz && \
    tar xvzf /opt/${R_VERSION}.tar.gz -C /opt/ && \
    cd /opt/${R_VERSION};./configure;make;make install && \
    rm /opt/${R_VERSION}.tar.gz

# Install NLopt
ENV NLOPT_VERSION="2.4.2"
RUN curl -fsSL http://ab-initio.mit.edu/nlopt/nlopt-${NLOPT_VERSION}.tar.gz -o /opt/nlopt-${NLOPT_VERSION}.tar.gz && \
    tar xvzf /opt/nlopt-${NLOPT_VERSION}.tar.gz -C /opt/ && \
    cd /opt/nlopt-${NLOPT_VERSION};./configure;make;make install && \
    rm /opt/nlopt-${NLOPT_VERSION}.tar.gz

# Install core R dependencies
RUN echo "r <- getOption('repos'); r['CRAN'] <- 'https://ftp.acc.umu.se/mirror/CRAN/'; options(repos = r);" > ~/.Rprofile && \
    Rscript -e "install.packages('statmod',dependencies=TRUE)" && \
    Rscript -e "install.packages('data.table',dependencies=TRUE)" && \
    Rscript -e "install.packages('gplots',dependencies=TRUE)" && \
    Rscript -e "install.packages('methods',dependencies=TRUE)"

# Install R Bioconductor packages
RUN echo 'source("https://bioconductor.org/biocLite.R")' > /opt/packages.r && \
    echo 'biocLite()' >> /opt/packages.r && \
    echo 'biocLite(c("limma", "edgeR"))' >> /opt/packages.r && \
    Rscript /opt/packages.r && \
    mkdir /usr/local/lib/R/site-library

# Install numpy and HTSeq
RUN pip install numpy && pip install htseq

# Install NGI Visualizations
RUN pip install --upgrade git+https://github.com/NationalGenomicsInfrastructure/ngi_visualizations.git

# Install MultiQC
# ENV MULTIQC_VERSION v0.9
ENV MULTIQC_VERSION master
RUN pip install git+git://github.com/ewels/MultiQC.git@$MULTIQC_VERSION
