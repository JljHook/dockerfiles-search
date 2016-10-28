FROM postgres:9.6

MAINTAINER George Drak <sonicgd@gmail.com>

WORKDIR /usr/local/src

RUN apt-get update && apt-get install -y \
    gcc \
    jq \
    make \
    postgresql-contrib-9.6 \
    postgresql-server-dev-9.6 \
    wget \
    && wget -O- $(wget -O- https://api.github.com/repos/dalibo/powa-archivist/releases/latest|jq -r '.tarball_url') | tar -xzf - \
    && wget -O- $(wget -O- https://api.github.com/repos/dalibo/pg_qualstats/releases/latest|jq -r '.tarball_url') | tar -xzf - \
    && wget -O- $(wget -O- https://api.github.com/repos/dalibo/pg_stat_kcache/releases/latest|jq -r '.tarball_url') | tar -xzf - \
    && wget -O- $(wget -O- https://api.github.com/repos/dalibo/hypopg/releases/latest|jq -r '.tarball_url') | tar -xzf - \
    && wget -O- $(wget -O- https://api.github.com/repos/rjuju/pg_track_settings/releases/latest|jq -r '.tarball_url') | tar -xzf - \
    && for f in $(ls); do cd $f; make install; cd ..; rm -rf $f; done \
    && apt-get purge -y --auto-remove gcc jq make postgresql-server-dev-9.6 wget \
    && rm -rf /var/lib/apt/lists/*

# configure powa-archivist
ADD setup_powa-archivist.sh /docker-entrypoint-initdb.d/
ADD install_all.sql /usr/local/src/