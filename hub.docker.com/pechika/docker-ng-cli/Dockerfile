FROM node:6.6

# prepare a user which runs everything locally! - required in child images!
RUN useradd --user-group --create-home --shell /bin/false app

ENV HOME=/work
WORKDIR $HOME

RUN npm install -g @angular/cli && npm cache clean
