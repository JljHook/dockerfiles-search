FROM ubuntu:18.04
MAINTAINER Miguel Brown (brownm28@email.chop.edu)

ENV GATK4_VERSION 4.0.5.2

RUN apt update && apt install -y openjdk-8-jdk python wget unzip ; \
wget https://github.com/broadinstitute/gatk/releases/download/${GATK4_VERSION}/gatk-${GATK4_VERSION}.zip; \
unzip gatk-${GATK4_VERSION}.zip; \
mv gatk-${GATK4_VERSION}/gatk* . && rm -rf gatk-${GATK4_VERSION}*; \
apt remove -y  wget