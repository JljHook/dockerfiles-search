# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

FROM ubuntu:16.04 AS build-simulator-core

RUN echo 'mysql-server mysql-server/root_password password root' | debconf-set-selections; \
    echo 'mysql-server mysql-server/root_password_again password root' | debconf-set-selections;

RUN apt-get -y update && apt-get install -y \
    genisoimage \
    libffi-dev \
    libssl-dev \
    sudo \
    ipmitool \
    maven \
    netcat \
    openjdk-8-jdk \
    python-dev \
    python-setuptools \
    python-pip \
    python-mysql.connector \
    supervisor \
    wget \
    nginx \
    jq \
    mysql-server \
    openssh-client \
    && apt-get clean all \
    && rm -rf /var/lib/apt/lists/*;

# TODO: check if and why this is needed
RUN mkdir -p /root/.ssh \
    && chmod 0700 /root/.ssh \
    && ssh-keygen -t rsa -N "" -f id_rsa.cloud

RUN mkdir -p /var/run/mysqld; \
    chown mysql /var/run/mysqld; \
    echo '''sql_mode = "STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"''' >> /etc/mysql/mysql.conf.d/mysqld.cnf

RUN find /var/lib/mysql -type f -exec touch {} \; && (/usr/bin/mysqld_safe &); sleep 30; mysqladmin -u root -proot password ''

RUN wget https://github.com/apache/cloudstack/archive/4.11.2.0.tar.gz -O /opt/cloudstack.tar.gz; \
    mkdir -p /opt/cloudstack; \
    tar xvzf /opt/cloudstack.tar.gz -C /opt/cloudstack --strip-components=1

WORKDIR /opt/cloudstack

RUN mvn -Pdeveloper -Dsimulator -DskipTests clean install
RUN mvn -Pdeveloper -Dsimulator dependency:go-offline
RUN mvn -pl client jetty:run -Dsimulator -Djetty.skip -Dorg.eclipse.jetty.annotations.maxWait=120

RUN (/usr/bin/mysqld_safe &); \
    sleep 5; \
    mvn -Pdeveloper -pl developer -Ddeploydb; \
    mvn -Pdeveloper -pl developer -Ddeploydb-simulator; \
    MARVIN_FILE=$(find /opt/cloudstack/tools/marvin/dist/ -name "Marvin*.tar.gz"); \
    pip install $MARVIN_FILE;

COPY zones.cfg /opt/zones.cfg
COPY nginx_default.conf /etc/nginx/sites-available/default
RUN pip install cs

FROM build-simulator-core

ENV KAFKA_ACKS=all
ENV KAFKA_WRITE_RETRIES=1
ENV KAFKA_TOPIC=cs

COPY run.sh /opt/run.sh
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
RUN mkdir -p ./client/target/classes/META-INF/cloudstack/core
COPY spring-event-bus-context.xml /opt/cloudstack/client/target/classes/META-INF/cloudstack/core/spring-event-bus-context.xml

EXPOSE 8888 8080 8096

CMD ["/usr/bin/supervisord"]
