####################################################################################
# Creates a container with OpenJDK 8
####################################################################################

# Command reference:
# docker build -t avanzabank/ubuntu-openjdk8:0.1.2 -t avanzabank/ubuntu-openjdk8:latest .
# docker run -it avanzabank/ubuntu-openjdk8 /bin/bash


FROM ubuntu:18.04

ENV LANG C.UTF-8

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    openjdk-8-jdk=8u181-b13-0ubuntu0.18.04.1 && \
    rm -rf /var/lib/apt/lists

WORKDIR /opt
