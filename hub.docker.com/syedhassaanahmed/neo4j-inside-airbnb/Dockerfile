FROM neo4j AS build
ARG FILE_LIMIT_MB=2
ENV FILE_LIMIT_MB=${FILE_LIMIT_MB}
ENV NEO4J_USERNAME=neo4j
ENV NEO4J_PASSWORD=temp-neo4j-password
ADD --chown=neo4j:neo4j https://raw.githubusercontent.com/syedhassaanahmed/neo4j-datasets/master/neo4j-start.sh ./
ADD --chown=neo4j:neo4j https://raw.githubusercontent.com/johnymontana/neo4j-datasets/master/airbnb/src/import/import.cypher ./
COPY import.sh ./
RUN apt-get update && \
    apt-get install -y curl && \
    chmod +x *.sh && ./neo4j-start.sh && \
    ./import.sh && \
    cp -r data/databases/graph.db /

FROM neo4j AS final
LABEL maintainer="Syed Hassaan Ahmed"
COPY --chown=neo4j:neo4j --from=build /graph.db/ /data/databases/graph.db