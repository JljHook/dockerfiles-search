FROM alt:sisyphus

MAINTAINER alt-cloud

RUN apt-get update && \
    apt-get install -y apache2; \
    rm -f /var/cache/apt/archives/*.rpm \
          /var/cache/apt/*.bin \
          /var/lib/apt/lists/*.*

EXPOSE 80

CMD ["/usr/sbin/httpd2", "-D", "NO_DETACH", "-k", "start"]
