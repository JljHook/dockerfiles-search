FROM            centos:centos6.7
MAINTAINER      hiroaki77
ENV             container docker
RUN             PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
RUN             yum update -y && yum clean all
RUN             rpm -ivh http://ftp.riken.jp/Linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN             rpm -ivh http://repo.webtatic.com/yum/el6/latest.rpm
RUN             rpm -ivh http://download1.rpmfusion.org/free/el/updates/6/x86_64/rpmfusion-free-release-6-1.noarch.rpm
RUN             rpm -ivh https://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
RUN             rpm -ivh http://mirror.yandex.ru/fedora/russianfedora/russianfedora/free/el/releases/6/Everything/x86_64/os/puias-release-6-2.R.noarch.rpm
RUN             yum install -y expect ntp wget vim-common vim-enhanced vim-minimal unzip zip vixie-cron sudo mailcap m4 autoconf automake make gcc rpm-build subversion gcc-c++ iftop net-snmp net-snmp-utils rrdtool pciutils bc screen nkf unzip unrar iftop lsof git hg
RUN             yum install -y python3 python3-tools libicu libtool-ltdl mcrypt unixODBC aspell net-snmp libtidy libxslt openssh-server
RUN             yum install -y php56w php56w-bcmath php56w-cli php56w-common php56w-dba php56w-devel php56w-embedded php56w-enchant php56w-fpm php56w-gd php56w-imap php56w-interbase php56w-intl php56w-ldap php56w-mbstring php56w-mcrypt php56w-mssql php56w-mysql php56w-odbc php56w-opcache php56w-pdo php56w-pgsql php56w-phpdbg php56w-process php56w-pspell php56w-recode php56w-snmp php56w-soap php56w-tidy php56w-xml php56w-xmlrpc httpd mod_ssl mysql-server
RUN             yum clean all
RUN             echo "<?php \n phpinfo();" > /var/www/html/index.php
RUN             mkdir -p /etc/skel/Maildir/{new,cur,tmp}
RUN             mkdir -p /etc/skel/{public_html,logs,tmp}
RUN             mkdir -p ~/Maildir/{new,cur,tmp}
RUN             echo "SELINUX=disabled">/etc/sysconfig/selinux
RUN             echo "SELINUXTYPE=targeted">>/etc/sysconfig/selinux
RUN             echo "SETLOCALDEFS=0">>/etc/sysconfig/selinux
RUN             chkconfig iptables off
RUN             chkconfig httpd on
RUN             chkconfig mysqld on
EXPOSE          80
EXPOSE          3306