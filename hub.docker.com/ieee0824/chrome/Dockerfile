FROM buildpack-deps:stretch-scm

RUN apt-get update \
    && apt-get install -y \
	    apt-transport-https \
	    ca-certificates \
	    gnupg \
	        --no-install-recommends \
	&& curl -sSL https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
	&& echo "deb [arch=amd64] https://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
	&& apt-get update \
	&& apt-get install -y \
	    google-chrome-stable \
	        --no-install-recommends \
	&& apt-get purge --auto-remove -y curl gnupg \
	&& rm -rf /var/lib/apt/lists/*

RUN groupadd -r chrome \
    && useradd -r -g chrome -G audio,video chrome \
    && mkdir -p /home/chrome \
    && chown -R chrome:chrome /home/chrome

USER chrome
EXPOSE 9222

ENTRYPOINT [ "google-chrome-stable", "--headless", "--disable-gpu", "--remote-debugging-address=0.0.0.0" ]

CMD [ "--user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:55.0) Gecko/20100101 Firefox/55.0", "--remote-debugging-port=9222" ]
