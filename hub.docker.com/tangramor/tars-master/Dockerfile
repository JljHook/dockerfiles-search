FROM centos

WORKDIR /root/

##修改镜像时区 
ENV TZ=Asia/Shanghai

ENV DBIP 127.0.0.1
ENV DBPort 3306
ENV DBUser root
ENV DBPassword password

# Mysql里tars用户的密码，缺省为tars2015
ENV DBTarsPass tars2015

##安装
RUN yum -y install https://repo.mysql.com/mysql57-community-release-el7-11.noarch.rpm \
	&& yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
	&& yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm \
	&& yum -y install yum-utils && yum-config-manager --enable remi-php72 \
	&& yum --enablerepo=mysql80-community -y install git gcc gcc-c++ make wget cmake mysql mysql-devel unzip iproute which glibc-devel flex bison ncurses-devel protobuf-devel zlib-devel kde-l10n-Chinese glibc-common hiredis-devel rapidjson-devel boost boost-devel redis php php-cli php-devel php-mcrypt php-gd php-curl php-mysql php-zip php-fileinfo php-phpiredis \
	&& ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
	&& localedef -c -f UTF-8 -i zh_CN zh_CN.utf8 \
	&& sed -i "s@;date.timezone =@date.timezone = ${TZ}@" /etc/php.ini \
	&& sed -i "s@AllowOverride None@AllowOverride All@g" /etc/httpd/conf/httpd.conf \
	# 安装Mysql8 C++ Connector
	&& wget -c -t 0 https://dev.mysql.com/get/Downloads/Connector-C++/mysql-connector-c++-8.0.11-linux-el7-x86-64bit.tar.gz \
	&& tar zxf mysql-connector-c++-8.0.11-linux-el7-x86-64bit.tar.gz && cd mysql-connector-c++-8.0.11-linux-el7-x86-64bit \
	&& cp -Rf include/jdbc/* /usr/include/mysql/ && cp -Rf include/mysqlx/* /usr/include/mysql/ && cp -Rf lib64/* /usr/lib64/mysql/ \
	&& cd /root && rm -rf mysql-connector* \
	# 获取最新TARS源码
	&& wget -c -t 0 https://github.com/Tencent/Tars/archive/master.zip -O master.zip \
	&& unzip -a master.zip && mv Tars-master Tars && rm -f /root/master.zip \
	&& mkdir -p /usr/local/mysql && ln -s /usr/lib64/mysql /usr/local/mysql/lib && ln -s /usr/include/mysql /usr/local/mysql/include && echo "/usr/local/mysql/lib/" >> /etc/ld.so.conf && ldconfig \
	&& cd /usr/local/mysql/lib/ && rm -f libmysqlclient.a && ln -s libmysqlclient.so.*.*.* libmysqlclient.a \
	&& cd /root/Tars/cpp/thirdparty && wget -c -t 0 https://github.com/Tencent/rapidjson/archive/master.zip -O master.zip \
	&& unzip -a master.zip && mv rapidjson-master rapidjson && rm -f master.zip \
	&& mkdir -p /data && chmod u+x /root/Tars/cpp/build/build.sh \
	# 以下对源码配置进行mysql8对应的修改
	&& sed -i '11s/rt/rt crypto ssl/' /root/Tars/cpp/framework/CMakeLists.txt && sed -i '20s/5.1.14/8.0.11/' /root/Tars/web/pom.xml \
	&& sed -i '38 a\\t<jaxb-ap.version>2.3.0</jaxb-ap.version>' /root/Tars/web/pom.xml \
	&& sed -i '303 a\\t<dependency>\n\t\t<groupId>javax.xml.bind</groupId>\n\t\t<artifactId>jaxb-api</artifactId>\n\t\t<version>${jaxb-ap.version}</version>\n\t</dependency>' /root/Tars/web/pom.xml \
	&& sed -i '25s/org.gjt.mm.mysql.Driver/com.mysql.cj.jdbc.Driver/' /root/Tars/web/src/main/resources/conf-spring/spring-context-datasource.xml \
	&& sed -i '26s/convertToNull/CONVERT_TO_NULL/' /root/Tars/web/src/main/resources/conf-spring/spring-context-datasource.xml \
	# 开始构建
	&& cd /root/Tars/cpp/build/ && ./build.sh all \
	&& ./build.sh install \
	&& cd /root/Tars/cpp/build/ && make framework-tar \
	&& make tarsstat-tar && make tarsnotify-tar && make tarsproperty-tar && make tarslog-tar && make tarsquerystat-tar && make tarsqueryproperty-tar \
	&& mkdir -p /usr/local/app/tars/ && cp /root/Tars/cpp/build/framework.tgz /usr/local/app/tars/ && cp /root/Tars/cpp/build/t*.tgz /root/ \
	&& cd /usr/local/app/tars/ && tar xzfv framework.tgz && rm -rf framework.tgz \
	&& mkdir -p /usr/local/app/patchs/tars.upload \
	&& cd /tmp && curl -fsSL https://getcomposer.org/installer | php \
	&& chmod +x composer.phar && mv composer.phar /usr/local/bin/composer \
	&& cd /root/Tars/php/tars-extension/ && phpize --clean && phpize \
	&& ./configure --enable-phptars --with-php-config=/usr/bin/php-config && make && make install \
	&& echo "extension=phptars.so" > /etc/php.d/phptars.ini \
	# 安装PHP swoole模块
	&& cd /root && wget -c -t 0 https://github.com/swoole/swoole-src/archive/v2.2.0.tar.gz \
	&& tar zxf v2.2.0.tar.gz && cd swoole-src-2.2.0 && phpize && ./configure && make && make install \
	&& echo "extension=swoole.so" > /etc/php.d/swoole.ini \
	&& cd /root && rm -rf v2.2.0.tar.gz swoole-src-2.2.0 \
	&& mkdir -p /root/phptars && cp -f /root/Tars/php/tars2php/src/tars2php.php /root/phptars \
	# 获取并安装JDK
	&& mkdir -p /root/init && cd /root/init/ \
	&& wget -c -t 0 --header "Cookie: oraclelicense=accept" -c --no-check-certificate http://download.oracle.com/otn-pub/java/jdk/10.0.2+13/19aef61b38124481863b1413dce1855f/jdk-10.0.2_linux-x64_bin.rpm \
	&& rpm -ivh /root/init/jdk-10.0.2_linux-x64_bin.rpm && rm -rf /root/init/jdk-10.0.2_linux-x64_bin.rpm \
	&& echo "export JAVA_HOME=/usr/java/jdk-10.0.2" >> /etc/profile \
	&& echo "CLASSPATH=\$JAVA_HOME/lib/dt.jar:\$JAVA_HOME/lib/tools.jar" >> /etc/profile \
	&& echo "PATH=\$JAVA_HOME/bin:\$PATH" >> /etc/profile \
	&& echo "export PATH JAVA_HOME CLASSPATH" >> /etc/profile \
	&& cd /usr/local/ && wget -c -t 0 http://mirror.bit.edu.cn/apache/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz \
	&& tar zxvf apache-maven-3.5.4-bin.tar.gz && echo "export MAVEN_HOME=/usr/local/apache-maven-3.5.4/" >> /etc/profile \
	# 设置阿里云maven镜像
	# && sed -i '/<mirrors>/a\\t<mirror>\n\t\t<id>nexus-aliyun<\/id>\n\t\t<mirrorOf>*<\/mirrorOf>\n\t\t<name>Nexus aliyun<\/name>\n\t\t<url>http:\/\/maven.aliyun.com\/nexus\/content\/groups\/public<\/url>\n\t<\/mirror>' /usr/local/apache-maven-3.5.4/conf/settings.xml \
	&& echo "export PATH=\$PATH:\$MAVEN_HOME/bin" >> /etc/profile && source /etc/profile && mvn -v \
	&& rm -rf apache-maven-3.5.4-bin.tar.gz  \
	&& cd /usr/local/ && wget -c -t 0 http://caucho.com/download/resin-4.0.56.tar.gz && tar zxvf resin-4.0.56.tar.gz && mv resin-4.0.56 resin && rm -rf resin-4.0.56.tar.gz \
	&& source /etc/profile && cd /root/Tars/java && mvn clean install && mvn clean install -f core/client.pom.xml && mvn clean install -f core/server.pom.xml \
	&& cd /root/Tars/web/ && source /etc/profile && mvn clean package \
	&& cp /root/Tars/build/conf/resin.xml /usr/local/resin/conf/ \
	&& sed -i 's/servlet-class="com.caucho.servlets.FileServlet"\/>/servlet-class="com.caucho.servlets.FileServlet">\n\t<init>\n\t\t<character-encoding>utf-8<\/character-encoding>\n\t<\/init>\n<\/servlet>/g' /usr/local/resin/conf/app-default.xml \
	&& sed -i 's/<page-cache-max>1024<\/page-cache-max>/<page-cache-max>1024<\/page-cache-max>\n\t\t<character-encoding>utf-8<\/character-encoding>/g' /usr/local/resin/conf/app-default.xml \
	&& cp /root/Tars/web/target/tars.war /usr/local/resin/webapps/ \
	&& mkdir -p /root/sql && cp -rf /root/Tars/cpp/framework/sql/* /root/sql/ \
	&& rm -rf /root/Tars \
	&& yum clean all && rm -rf /var/cache/yum
	
ENV JAVA_HOME /usr/java/jdk-10.0.2

ENV MAVEN_HOME /usr/local/apache-maven-3.5.4

# 是否将Tars系统进程的data目录挂载到外部存储，缺省为false以支持windows下使用
ENV MOUNT_DATA false

# 网络接口名称，如果运行时使用 --net=host，宿主机网卡接口可能不叫 eth0
ENV INET_NAME eth0

# 中文字符集支持
ENV LC_ALL "zh_CN.UTF-8"

VOLUME ["/data"]

##拷贝资源
COPY install.sh /root/init/
COPY entrypoint.sh /sbin/

ADD confs /root/confs

ADD pid1-0.1.0-amd64 /sbin/pid1
RUN chmod 755 /sbin/pid1 /sbin/entrypoint.sh
ENTRYPOINT [ "/sbin/pid1" ]
CMD bash -c '/sbin/entrypoint.sh start'

#Expose ports
EXPOSE 8080
EXPOSE 80
