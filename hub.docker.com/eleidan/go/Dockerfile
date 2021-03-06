FROM eleidan/buildpack-deps:jessie

LABEL maintainer="Oleg Kulyk" \
      description="Go 1.8.3 based on Debian GNU/Linux 8 (jessie)" \
      init="docker run -it --rm -v $(pwd):/home/phantom/app eleidan/go:1.8.3-jessie docker-init.sh" \
      run="docker run -it --rm -v $(pwd):/home/phantom/app eleidan/go:1.8.3-jessie" \
      version="2017-06-20"

ENV GOLANG_VERSION=1.8.3 \
    SERVICE_NAME="go:1.8.3-jessie"

RUN set -eux; \
  	dpkgArch="$(dpkg --print-architecture)"; \
  	case "${dpkgArch##*-}" in \
  		ppc64el) goRelArch='linux-ppc64le'; goRelSha256='e5fb00adfc7291e657f1f3d31c09e74890b5328e6f991a3f395ca72a8c4dc0b3' ;; \
  		i386) goRelArch='linux-386'; goRelSha256='ff4895eb68fb1daaec41c540602e8bb4c1e8bb2f0e7017367171913fc9995ed2' ;; \
  		s390x) goRelArch='linux-s390x'; goRelSha256='e2ec3e7c293701b57ca1f32b37977ac9968f57b3df034f2cc2d531e80671e6c8' ;; \
  		armhf) goRelArch='linux-armv6l'; goRelSha256='3c30a3e24736ca776fc6314e5092fb8584bd3a4a2c2fa7307ae779ba2735e668' ;; \
  		amd64) goRelArch='linux-amd64'; goRelSha256='1862f4c3d3907e59b04a757cfda0ea7aa9ef39274af99a784f5be843c80c6772' ;; \
  		*) goRelArch='src'; goRelSha256='33daf4c03f86120fdfdc66bddf6bfff4661c7ca11c5da473e537f4d69b470e57'; \
  			echo >&2; echo >&2 "warning: current architecture ($dpkgArch) does not have a corresponding Go binary release; will be building from source"; echo >&2 ;; \
  	esac; \
  	\
  	url="https://golang.org/dl/go${GOLANG_VERSION}.${goRelArch}.tar.gz"; \
  	wget -O go.tgz "$url"; \
  	echo "${goRelSha256} *go.tgz" | sha256sum -c -; \
  	tar -C /usr/local -xzf go.tgz; \
  	rm go.tgz; \
  	\
  	if [ "$goRelArch" = 'src' ]; then \
  		echo >&2; \
  		echo >&2 'error: UNIMPLEMENTED'; \
  		echo >&2 'TODO install golang-any from jessie-backports for GOROOT_BOOTSTRAP (and uninstall after build)'; \
  		echo >&2; \
  		exit 1; \
  	fi; \
  	\
  	export PATH="/usr/local/go/bin:$PATH"; \
  	go version

ENV GOPATH="$HOME/app" \
    PATH="$GOPATH/bin:/usr/local/go/bin:$PATH"

WORKDIR $GOPATH

COPY go-wrapper docker-init.sh /usr/local/bin/
RUN chmod +x \
      /usr/local/bin/docker-init.sh\
      /usr/local/bin/go-wrapper

### Needed for the docker-init.sh script
COPY recipes $HOME/recipes
