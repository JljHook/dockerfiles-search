FROM selenium/base:latest
MAINTAINER Jesse Bye <jbye@riskalyze.com>

#========================
# Selenium Configuration
#========================

EXPOSE 4444

ENV GRID_THROW_ON_CAPABILITY_NOT_PRESENT true
ENV GRID_NEW_SESSION_WAIT_TIMEOUT -1
ENV GRID_JETTY_MAX_THREADS -1
ENV GRID_NODE_POLLING  5000
ENV GRID_CLEAN_UP_CYCLE 5000
ENV GRID_TIMEOUT 300000
ENV GRID_BROWSER_TIMEOUT 0
ENV GRID_MAX_SESSION 5
ENV GRID_UNREGISTER_IF_STILL_DOWN_AFTER 300000

COPY generate_config /opt/selenium/generate_config
COPY entry_point.sh /opt/bin/entry_point.sh
RUN /opt/selenium/generate_config > /opt/selenium/config.json

USER seluser

CMD ["/opt/bin/entry_point.sh"]
