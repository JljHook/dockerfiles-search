FROM microsoft/aspnetcore:2.0 AS finalbase
RUN apt update
RUN apt install -y sysstat iproute2 procps sudo
RUN apt install python-pip -y
RUN pip install psutil
WORKDIR /opt/acmnodemanager
RUN touch /etc/sudoers

