FROM ruby:2.3.1-alpine
MAINTAINER Gordon Chan <github.com/gchan>

# Copied from https://github.com/docker-library/httpd/blob/master/2.4/alpine/Dockerfile
# ensure www-data user exists
RUN set -x \
  && addgroup -g 82 -S www-data \
  && adduser -u 82 -D -S -G www-data www-data
# 82 is the standard uid/gid for "www-data" in Alpine
# http://git.alpinelinux.org/cgit/aports/tree/main/apache2/apache2.pre-install?h=v3.3.2
# http://git.alpinelinux.org/cgit/aports/tree/main/lighttpd/lighttpd.pre-install?h=v3.3.2
# http://git.alpinelinux.org/cgit/aports/tree/main/nginx-initscripts/nginx-initscripts.pre-install?h=v3.3.2

ENV HTTPD_PREFIX /usr/local/apache2
ENV PATH $HTTPD_PREFIX/bin:$PATH
RUN mkdir -p "$HTTPD_PREFIX" \
  && chown www-data:www-data "$HTTPD_PREFIX"
WORKDIR $HTTPD_PREFIX

ENV HTTPD_VERSION 2.4.23
ENV HTTPD_SHA1 5101be34ac4a509b245adb70a56690a84fcc4e7f
ENV HTTPD_BZ2_URL https://www.apache.org/dist/httpd/httpd-$HTTPD_VERSION.tar.bz2

# see https://httpd.apache.org/docs/2.4/install.html#requirements
RUN set -x \
  && runDeps=' \
    apr-dev \
    apr-util-dev \
  ' \
  && apk add --no-cache --virtual .build-deps \
    $runDeps \
    ca-certificates \
    gcc \
    gnupg \
    libc-dev \
    make \
    openssl \
    openssl-dev \
    pcre-dev \
    tar \
  \
  && wget -O httpd.tar.bz2 "$HTTPD_BZ2_URL" \
  && echo "$HTTPD_SHA1 *httpd.tar.bz2" | sha1sum -c - \
# see https://httpd.apache.org/download.cgi#verify
  && wget -O httpd.tar.bz2.asc "$HTTPD_BZ2_URL.asc" \
  && export GNUPGHOME="$(mktemp -d)" \
  && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys A93D62ECC3C8EA12DB220EC934EA76E6791485A8 \
  && gpg --batch --verify httpd.tar.bz2.asc httpd.tar.bz2 \
  && rm -r "$GNUPGHOME" httpd.tar.bz2.asc \
  \
  && mkdir -p src \
  && tar -xvf httpd.tar.bz2 -C src --strip-components=1 \
  && rm httpd.tar.bz2 \
  && cd src \
  \
  && ./configure \
    --prefix="$HTTPD_PREFIX" \
    --enable-mods-shared=reallyall \
  && make -j"$(getconf _NPROCESSORS_ONLN)" \
  && make install \
  \
  && cd .. \
  && rm -r src \
  \
  && sed -ri \
    -e 's!^(\s*CustomLog)\s+\S+!\1 /proc/self/fd/1!g' \
    -e 's!^(\s*ErrorLog)\s+\S+!\1 /proc/self/fd/2!g' \
    "$HTTPD_PREFIX/conf/httpd.conf" \
  \
  && runDeps="$runDeps $( \
    scanelf --needed --nobanner --recursive /usr/local \
      | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
      | sort -u \
      | xargs -r apk info --installed \
      | sort -u \
  )" \
  && apk add --virtual .httpd-rundeps $runDeps \
  && apk del .build-deps

COPY ./httpd-foreground /usr/local/bin/
COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf

RUN mkdir -p /httpoxy
COPY . /httpoxy

CMD ["httpd-foreground"]