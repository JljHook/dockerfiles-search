FROM frolvlad/alpine-oraclejdk8:latest
MAINTAINER Paul Ambrose "pambrose@mac.com"

WORKDIR /prometheus-proxy

COPY . /prometheus-proxy/

RUN ./mvnw -DskipTests=true clean package && \
    rm -rf /root/.m2 .mvn etc src target/*.jar target/protoc* target/maven* target/generated* target/archive* target/test-classes

EXPOSE 8083
expose 8093

ENTRYPOINT ["/prometheus-proxy/target/bin/agent.sh"]
