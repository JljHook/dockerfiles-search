FROM ubuntu
MAINTAINER Sergey Dolin <sergey@s4y.solutions>
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
RUN echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.4.list
RUN apt-get update \
  && apt-get install -y  mongodb-org \
	  && rm -rf /var/lib/apt/lists/*
		EXPOSE 27017
		CMD /usr/bin/mongod --dbpath /mongodb --bind_ip 0.0.0.0 --storageEngine mmapv1
