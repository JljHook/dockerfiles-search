FROM openjdk:8-jdk-slim as builder
LABEL maintainer="wout.slakhorst@nuts.nl"
COPY / .
RUN ./gradlew jar

FROM openjdk:8-jre-slim
RUN mkdir /opt/nuts
COPY lib/corda.jar /opt/nuts/corda.jar
COPY docker/files/certificates/network-root-truststore.jks /opt/nuts/certificates/network-root-truststore.jks
COPY --from=builder contract/build/libs/contract-0.11.2.jar /opt/nuts/cordapps/contract-0.11.2.jar
COPY --from=builder flows/build/libs/flows-0.11.2.jar /opt/nuts/cordapps/flows-0.11.2.jar
WORKDIR /opt/nuts
ENTRYPOINT ["/usr/local/openjdk-8/bin/java"]
CMD ["-jar", "/opt/nuts/corda.jar", "--network-root-truststore-password=changeit", "--log-to-console"]
EXPOSE 7886 7887 7888 2222