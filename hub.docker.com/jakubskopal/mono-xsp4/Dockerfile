FROM jakubskopal/mono:4.5

RUN apt-get update && apt-get install -y \
		mono-xsp4 \
	&& rm -rf /var/lib/apt/lists/*

RUN mkdir -p /usr/local/bin

COPY xsp4-wrapper.sh /usr/local/bin

