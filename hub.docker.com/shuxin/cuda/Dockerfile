# Start with Ubuntu base image 
FROM ubuntu:14.04 
MAINTAINER Xinshu<605319493@qq.com> 

# Add NVIDIA Docker information 
LABEL com.nvidia.volumes.needed="nvidia_driver"
LABEL com.nvidia.cuda.version="7.0"

# Install wget and build-essential 
RUN apt-get update && apt-get install -y \
  build-essential \
  wget

# Change to the /tmp directory 
RUN cd /tmp && \
# Download run file
  wget http://developer.download.nvidia.com/compute/cuda/7_0/Prod/local_installers/cuda_7.0.28_linux.run -o /dev/null && \ 
# Make the run file executable and extract 
  chmod +x cuda_*_linux.run && ./cuda_*_linux.run -extract=`pwd` && \ 
# Install CUDA drivers (silent, no kernel) 
  ./NVIDIA-Linux-x86_64-*.run -s --no-kernel-module && \ 
# Install toolkit (silent)  
  ./cuda-linux64-rel-*.run -noprompt | cat > /dev/null && \ 
# Clean up 
  rm -rf * 

# Add to path 
ENV PATH=/usr/local/cuda/bin:$PATH \ 
LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
