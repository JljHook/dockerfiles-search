# Copyright 2013 Thatcher Peskens
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM debian:stretch-slim

LABEL MAINTAINER "Erik Tiengo <erik.tiengo@gisup.xyz>"

# Install required packages and remove the apt packages cache when done.

RUN apt-get update && \
    apt-get upgrade -y && \ 	
    apt-get install -y \
	git \
	python3 \
	python3-dev \
	python3-setuptools \
	python3-pip \
	nginx \
	supervisor \
	sqlite3 && \
	pip3 install -U pip setuptools && \
	rm -rf /var/lib/apt/lists/*

# install uwsgi now because it takes a little while
RUN pip3 install uwsgi

# setup all the configfiles
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
COPY nginx-app.conf /etc/nginx/sites-available/default
COPY supervisor-app.conf /etc/supervisor/conf.d/

# COPY requirements.txt and RUN pip install BEFORE adding the rest of your code, this will cause Docker's caching mechanism
# to prevent re-installing (all your) dependencies when you made a change a line or two in your app.

COPY docker4django/requirements.txt /home/docker/code/docker4django/
RUN pip3 install -r /home/docker/code/docker4django/requirements.txt

# add (the rest of) our code
COPY . /home/docker/code/

# set current directory
WORKDIR /home/docker/code/docker4django
# launch migrate to generate local database
RUN python3 manage.py migrate
# set current directory
WORKDIR /home/docker/code

EXPOSE 80
CMD ["supervisord", "-n"]
