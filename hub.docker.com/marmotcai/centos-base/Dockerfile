FROM centos:7 AS base
MAINTAINER marmotcai "marmotcai@163.com"

RUN yum clean all && rm -rf /etc/yum.repos.d/*.repo
RUN     curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo && \
        curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

RUN     sed -i '/aliyuncs/d' /etc/yum.repos.d/CentOS-Base.repo && \
        sed -i '/aliyuncs/d' /etc/yum.repos.d/epel.repo && \
        sed -i 's/$releasever/7/g' /etc/yum.repos.d/CentOS-Base.repo

RUN yum clean all && \
        yum makecache fast

# yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# subscription-manager repos --enable "rhel-*-optional-rpms" --enable "rhel-*-extras-rpms"  --enable "rhel-ha-for-rhel-*-server-rpms"

RUN yum update -y && yum install -y vim wget sudo git openssh openssh-server openssh-clients

RUN mkdir /var/run/sshd
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN sed -i 's/#UseDNS yes/UseDNS no/g' /etc/ssh/sshd_config
RUN sed -i 's/GSSAPIAuthentication yes/GSSAPIAuthentication no/g' /etc/ssh/sshd_config
RUN /bin/echo 'root:112233' | chpasswd
RUN /bin/sed -i 's/.*session.*required.*pam_loginuid.so.*/session optional pam_loginuid.so/g' /etc/pam.d/sshd
RUN /bin/echo -e "LANG=\"en_US.UTF-8\"" > /etc/default/local
# RUN /bin/echo -e "LANG=\"zh_CN.UTF-8\"" > /etc/default/local

RUN systemctl enable sshd.service

#向容器安装中文编码支持
RUN  yum -y install kde-l10n-Chinese telnet && \
     yum -y reinstall glibc-common &&\
     yum clean all  && \
     localedef -c -f UTF-8 -i zh_CN zh_CN.utf8
# ENV LC_ALL zh_CN.utf8

EXPOSE 22

CMD /usr/sbin/sshd -D
# CMD /usr/sbin/init
