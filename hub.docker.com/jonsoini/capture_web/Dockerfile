FROM python:3.6.4

WORKDIR /tmp

RUN sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' && \
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    apt-get update && \
    apt-get install -y google-chrome-stable

RUN apt-get install -y unzip && \
    curl -LO https://chromedriver.storage.googleapis.com/76.0.3809.68/chromedriver_linux64.zip && \
    unzip chromedriver_linux64.zip && \
    mv chromedriver /usr/local/bin/

RUN apt-get -y install fonts-ipafont-gothic fonts-ipafont-mincho
ENV LANG=en_US.UTF-8
ENV LANGUAGE="en_US:en"

ADD requirements.txt /tmp/
RUN pip install -r requirements.txt

RUN mkdir -p /tmp/screenshot
WORKDIR /tmp/screenshot
ADD screenshot.py /tmp/

ENTRYPOINT ["python", "/tmp/screenshot.py"]
CMD ["--help"]
