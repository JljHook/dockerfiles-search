FROM rubylang/ruby:2.6-bionic

# Install Requirement packages for installing node and yarn
RUN apt-get update \
  && apt-get install -y curl gnupg \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN curl -sL https://deb.nodesource.com/setup_11.x | bash - \
  && curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
  && apt-get update \
  && apt-get install -y build-essential mariadb-client libmariadbclient-dev libcurl4 nodejs yarn tzdata \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/app
