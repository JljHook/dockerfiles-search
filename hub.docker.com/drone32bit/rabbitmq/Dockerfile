# Pull base image.
FROM patrickjahns/ubuntu-debootstrap:14.04

# Add files.
ADD rabbitmq-start /usr/local/bin/

# Install RabbitMQ.
RUN 	apt-get update && apt-get install -y wget && \
  	wget -qO - https://www.rabbitmq.com/rabbitmq-signing-key-public.asc | apt-key add - && \
  	echo "deb http://www.rabbitmq.com/debian/ testing main" > /etc/apt/sources.list.d/rabbitmq.list && \
  	apt-get update && \
  	DEBIAN_FRONTEND=noninteractive apt-get install -y rabbitmq-server && \
  	apt-get clean && apt-get autoremove && \
  	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
  	rm -rf /var/cache/apt/archives/*.deb && \
  	rabbitmq-plugins enable rabbitmq_management && \
  	echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config && \
  	chmod +x /usr/local/bin/rabbitmq-start

# Define environment variables.
ENV RABBITMQ_LOG_BASE /data/log
ENV RABBITMQ_MNESIA_BASE /data/mnesia

# Define mount points.
VOLUME ["/data/log", "/data/mnesia"]

# Define working directory.
WORKDIR /data

# Define default command.
CMD ["rabbitmq-start"]

# Expose ports.
EXPOSE 5672
EXPOSE 15672
