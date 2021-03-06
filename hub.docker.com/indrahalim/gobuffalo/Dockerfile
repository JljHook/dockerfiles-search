FROM golang:latest
EXPOSE 3000
ENV BP=$GOPATH/src/github.com/gobuffalo/buffalo
RUN go version

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash \
&& sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list' \
&& wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | apt-key add - \
&& apt-get update \
&& apt-get install -y -q build-essential nodejs sqlite3 libsqlite3-dev postgresql postgresql-contrib libpq-dev mysql-client vim \
&& rm -rf /var/lib/apt/lists/*

RUN service postgresql start && \
    su -c "psql -c \"ALTER USER postgres  WITH PASSWORD 'postgres';\"" - postgres

RUN go get -u github.com/golang/dep/cmd/dep \
&& go get -v -u github.com/gobuffalo/pop \
&& go get -v -u github.com/gobuffalo/buffalo-pop \
&& go get -v -u github.com/gobuffalo/packr/packr \
&& go get -v -u github.com/markbates/filetest \
&& go get -v -u github.com/markbates/grift \
&& go get -v -u github.com/markbates/refresh \
&& go get -u github.com/alecthomas/gometalinter \
&& gometalinter --install

RUN npm install -g --no-progress yarn \
&& yarn config set yarn-offline-mirror /npm-packages-offline-cache \
&& yarn config set yarn-offline-mirror-pruning true


RUN rm -rfv $BP && mkdir -p $BP
WORKDIR $BP
COPY . .
RUN make ci-deps && make install

# cache yarn packages to an offline mirror so they're faster to load. hopefully.
RUN grep -v '{{' ./generators/assets/webpack/templates/package.json.tmpl > package.json \
&& yarn install --no-progress

RUN buffalo version

WORKDIR $GOPATH/src

RUN ls -la /npm-packages-offline-cache