# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
# NOTE: DO *NOT* EDIT THIS FILE.  IT IS GENERATED.
# PLEASE UPDATE Dockerfile.txt INSTEAD OF THIS FILE
# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
FROM ymxhtest/ds-base:3.9.1
MAINTAINER ymxhtest <ymxhtest@163.com>

USER root

#========================
# 容器监听的端口
#========================
EXPOSE 4444

#========================
# selenium_hub配置
#========================
# As string, maps to "host"
ENV GRID_HUB_HOST 0.0.0.0
# hub port
ENV GRID_HUB_PORT 4444
ENV GRID_THROW_ON_CAPABILITY_NOT_PRESENT true
# In milliseconds
ENV GRID_NEW_SESSION_WAIT_TIMEOUT -1
ENV GRID_JETTY_MAX_THREADS -1
# In milliseconds
ENV GRID_NODE_POLLING 5000
# In milliseconds
ENV GRID_CLEAN_UP_CYCLE 5000
# In seconds
ENV GRID_TIMEOUT 180
# In seconds
ENV GRID_BROWSER_TIMEOUT 0
# In seconds
ENV GRID_MAX_SESSION 15
# In milliseconds
ENV GRID_UNREGISTER_IF_STILL_DOWN_AFTER 60000

COPY generate_config /opt/selenium/generate_config
COPY entry_point.sh /opt/bin/entry_point.sh
# 把generate_config内容转为config.json
RUN chmod +x /opt/selenium/generate_config \
  && chmod +x /opt/bin/entry_point.sh \
  && sh /opt/selenium/generate_config > /opt/selenium/config.json

RUN chown -R seluser:seluser /opt/selenium

USER seluser
  
# 执行entry_point.sh文件
CMD ["/opt/bin/entry_point.sh"]


