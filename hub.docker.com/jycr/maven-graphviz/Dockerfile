FROM maven:3.5.4-jdk-8-slim

RUN apt-get update && apt-get install -y \
           graphviz \
           ttf-freefont \
    && rm -rf /var/lib/apt/lists/*
