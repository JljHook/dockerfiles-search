FROM centos:latest
# 镜像的作者 
LABEL maintainer="<371583076@qq.com>" \
      name="Python 3.7" \
      author="xiao.li" \
      vendor="qiaodapei" \
      version="v0.1" \
      systemver="Centos7.6" \
      desc="pyhon37 or nginx" \
      build-date="20190422"   
### SET ENVIRONNEMENT
ENV LANG zh_CN.UTF-8 
COPY Resources /tmp/Resources
RUN localedef -v -c -i en_US -f UTF-8 zh_CN.UTF-8 >/dev/null 2>&1 &&\
    ln -nfs  /usr/share/zoneinfo/Asia/Shanghai /etc/localtime &&\
    grep -q 'zh_CN.utf8' /etc/locale.conf || sed -i -E 's/^LANG=.*/LANG="zh_CN.UTF-8"/' /etc/locale.conf &&\
    mkdir -p /root/env && mkdir /data &&\
    yum -y install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm &&\
    yum -y install http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm &&\
    yum -y install http://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm &&\
    yum install -y ntp yum-plugin-fastestmirror vim-enhanced  ntp wget bash-completion elinks lrzsz unix2dos dos2unix \
    git unzip python python-devel python-pip net-tools cronie gcc mysql-devel nginx zabbix-agent&&\
    yum -y localinstall /tmp/Resources/python37/*.rpm &&ln -s /usr/bin/python37 /usr/bin/python3 &&\
    curl https://bootstrap.pypa.io/get-pip.py -o /tmp/Resources/get-pip.py && python3 /tmp/Resources/get-pip.py &&\
    pip3 install virtualenv && virtualenv --no-site-packages /root/env && yum clean all &&  rm -fr /tmp/Resources  

VOLUME ["/sys/fs/cgroup"]

CMD ["/usr/sbin/init"]
 
