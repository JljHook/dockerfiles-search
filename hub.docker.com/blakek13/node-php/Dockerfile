FROM php:latest

# install curl and git
RUN apt-get -y update && apt-get install -y curl git

# install Node.js LTS
RUN curl -sL 'https://deb.nodesource.com/setup_6.x' | bash /dev/stdin

# install yarn package manager
RUN curl -sS 'https://dl.yarnpkg.com/debian/pubkey.gpg' | apt-key add -
RUN echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get -y update && apt-get install -y nodejs yarn
