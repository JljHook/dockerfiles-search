FROM codesignal/ubuntu-base:v5.0

# Install PHP & dependencies
RUN add-apt-repository ppa:ondrej/php \
  && apt-get -qq -y update \
  && apt-get install -y --no-install-recommends php7.2 \
  && apt-get install -y --no-install-recommends \
    php7.2-curl \
    php7.2-mysql \
    php7.2-xml \
  && rm -rf /var/lib/apt/lists/*

# Install PHPUnit
RUN wget -O phpunit https://phar.phpunit.de/phpunit-8.phar \
  && chmod +x phpunit \
  && mv phpunit /usr/local/bin/phpunit
