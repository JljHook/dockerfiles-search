FROM debian:jessie

LABEL maintainer="mike-sino"
LABEL version="1.0"

FROM python:3.6.0
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
RUN mkdir /code/db
WORKDIR /code
ADD ./init/requirements.txt /code/
RUN pip install -r requirements.txt 
ADD . /code/