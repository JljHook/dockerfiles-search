FROM bitnami/oraclelinux-extras-base:7-r386
LABEL maintainer "ygqygq2 <29ygq@sina.com>"

ENV BITNAMI_PKG_CHMOD="-R g+rwX" \
    HOME="/" \
    OS_ARCH="x86_64" \
    OS_FLAVOUR="ol-7" \
    OS_NAME="linux"

# Install required system packages and dependencies
RUN install_packages glibc hostname libgcc zlib
RUN . ./libcomponent.sh && component_unpack "java" "1.8.222-0" --checksum a90f4ce71ff9b392efdf61e6941117afa31d2d6528bd1a3cad1c6459bcac2b0b
RUN . ./libcomponent.sh && component_unpack "elasticsearch" "5.6.16-0" --checksum 4726400c203fbec0281f058bb6bfc3173741e43e76e8a036a1c86b116624ac43

COPY rootfs /
RUN /postunpack.sh
RUN chown -R 1001 /opt/bitnami/elasticsearch/config && chown -R 1001 /opt/bitnami/elasticsearch/plugins
ENV BITNAMI_APP_NAME="elasticsearch" \
    BITNAMI_IMAGE_VERSION="5.6.16-ol-7-r0" \
    ELASTICSEARCH_BIND_ADDRESS="" \
    ELASTICSEARCH_CLUSTER_HOSTS="" \
    ELASTICSEARCH_CLUSTER_MASTER_HOSTS="" \
    ELASTICSEARCH_CLUSTER_NAME="elasticsearch-cluster" \
    ELASTICSEARCH_HEAP_SIZE="1024m" \
    ELASTICSEARCH_IS_DEDICATED_NODE="no" \
    ELASTICSEARCH_MINIMUM_MASTER_NODES="" \
    ELASTICSEARCH_NODE_NAME="" \
    ELASTICSEARCH_NODE_PORT_NUMBER="9300" \
    ELASTICSEARCH_NODE_TYPE="master" \
    ELASTICSEARCH_PLUGINS="" \
    ELASTICSEARCH_PORT_NUMBER="9200" \
    LD_LIBRARY_PATH="/opt/bitnami/elasticsearch/jdk/lib:/opt/bitnami/elasticsearch/jdk/lib/server:$LD_LIBRARY_PATH" \
    NAMI_PREFIX="/.nami" \
    PATH="/opt/bitnami/java/bin:/opt/bitnami/elasticsearch/bin:$PATH"

EXPOSE 9200 9300

USER 1001
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "/run.sh" ]
