FROM debian:jessie

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
RUN echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" > /etc/apt/sources.list.d/mongodb-org-3.4.list
RUN apt-get update && apt-get install -y dnsutils mongodb-org-shell

COPY ["run.sh", "init.template.js", "task.template.js", "/"]

CMD ["/run.sh"]

ENTRYPOINT ["bash", "-c"]
