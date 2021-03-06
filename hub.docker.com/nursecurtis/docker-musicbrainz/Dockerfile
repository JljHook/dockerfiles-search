FROM lsiobase/alpine:3.6

# set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="sparklyballs"

# copy files required in build stage
COPY prebuilds/ /defaults/

# package versions
ARG BRAINZ_VER="v-2018-06-30"

# global environment settings
ENV BABEL_DISABLE_CACHE="1" \
HOME="/root" \
LANG="en_US.utf8" \
MBDATA="/data/import" \
PGCONF="/config" \
PGDATA="/data/dbase" \
UPDATE_SLAVE_LOGDIR="/config/log/musicbrainz" \
URL_ROOT="ftp://ftp.musicbrainz.org/pub/musicbrainz/data/fullexport"

RUN \
 echo "**** install build packages ****" && \
 apk add --no-cache --virtual=build-dependencies \
	db-dev \
	expat-dev \
	g++ \
	gcc \
	icu-dev \
	libxml2-dev \
	make \
	perl-dev && \
 echo "**** install runtime packages ****" && \
 apk add --no-cache \
	bzip2 \
	curl \
	db \
	expat \
	git \
	icu-libs \
	nginx \
	nodejs \
	patch \
	logrotate \
        redis \
	perl \
	perl-crypt-rijndael \
	perl-dbd-pg \
	perl-db_file \
	perl-net-ssleay \
	postgresql \
	postgresql-dev \
	postgresql-client \
	procps \
	tar \
	wget \
	rsync \
	openssh-client \
	yarn && \
 echo "**** fetch musicbrainz and install perl and node packages ****" && \
 mkdir -p \
	/app/musicbrainz && \
 curl -o \
 /tmp/musicbrainz.tar.gz -L \
	"https://github.com/metabrainz/musicbrainz-server/archive/${BRAINZ_VER}.tar.gz" && \
 tar xf \
 /tmp/musicbrainz.tar.gz -C \
	/app/musicbrainz --strip-components=1 && \
 sed -i 's#$MB_SERVER_ROOT/#$UPDATE_SLAVE_LOGDIR/#g' /app/musicbrainz/admin/cron/slave.sh && \
 cp /defaults/DBDefs.pm /app/musicbrainz/lib/DBDefs.pm && \
 cd /app/musicbrainz && \
 curl -L http://cpanmin.us | perl - App::cpanminus && \
 cpanm --installdeps --notest . && \
 cpanm --notest \
	Cache::Memcached::Fast \
	Cache::Memory \
	Catalyst::Plugin::Cache::HTTP \
	Catalyst::Plugin::StackTrace \
	Digest::MD5::File \
	FCGI \
	FCGI::ProcManager \
	Plack::Handler::Starlet \
	Plack::Middleware::Debug::Base \
	Server::Starter \
	Starlet \
	Starlet::Server \
	Term::Size::Any && \
 yarn install && \
 yarn cache clean && \
 ./script/compile_resources.sh && \
 echo "**** configure nginx ****" && \
 echo 'fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;' >> \
	/etc/nginx/fastcgi_params && \
 rm -f /etc/nginx/conf.d/default.conf && \
 echo "**** cleanup ****" && \
 apk del --purge \
	build-dependencies && \
 rm -rf \
	/root/.cpanm \
	/tmp/*

# add local files
COPY root/ /

# volumes and ports
VOLUME /config /data
EXPOSE 5000
