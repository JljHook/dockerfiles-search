FROM openjdk:8-jre-alpine

RUN apk add --no-cache --quiet \
    bash \
    curl

ENV NEO4J_SHA256=8a2a74f1270944d9b72f2af2c15cb350718e697af6e3800e67cb32a5d1605f6e \
    NEO4J_TARBALL=neo4j-community-3.3.2-unix.tar.gz \
    NEO4J_EDITION=community
    ARG NEO4J_URI=http://dist.neo4j.org/neo4j-community-3.3.2-unix.tar.gz

COPY ./local-package/* /tmp/

RUN curl --fail --silent --show-error --location --remote-name ${NEO4J_URI} \
    && echo "${NEO4J_SHA256}  ${NEO4J_TARBALL}" | sha256sum -csw - \
    && tar --extract --file ${NEO4J_TARBALL} --directory /var/lib \
    && mv /var/lib/neo4j-* /var/lib/neo4j \
    && rm ${NEO4J_TARBALL} \
    && mv /var/lib/neo4j/data /data \
    && ln -s /data /var/lib/neo4j/data

ENV PATH /var/lib/neo4j/bin:$PATH

WORKDIR /var/lib/neo4j

RUN curl --fail --silent --show-error --location -o /tmp/graph.dump  https://www.dropbox.com/s/33ysrq3mbta5pck/ecoli_21.5_v0.1.dump?dl=0 && \
    bin/neo4j-admin load --from=/tmp/graph.dump --database=graph.db --force && \
    rm /tmp/graph.dump

VOLUME /data

COPY docker-entrypoint.sh /docker-entrypoint.sh

EXPOSE 7474 7473 7687

ENV NEO4J_AUTH=none

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["neo4j"]
