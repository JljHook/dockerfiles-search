FROM ubuntu:latest

COPY tools /tools

ENV OSXCROSS_SDK_VERSION=10.13

ENV DEBIAN_FRONTEND=noninteractive
ENV PORTABLE=1

RUN dpkg --add-architecture i386 && \
	apt-get update -qq && \
	apt-get install -qq -y software-properties-common && \
	rm -rf /var/lib/apt/lists/* && \
	add-apt-repository ppa:longsleep/golang-backports && \
	apt-get update -qq && \
	apt-get install -qq --assume-yes apt-utils && \
	apt-get install -qq -y wget curl git \
		gcc g++ make \
		# for osx cross
		clang autotools-dev automake cmake libfuse-dev \
		# golang
		golang-go upx \
		# xar
		libxml2-dev openssl1.0 libssl1.0-dev \
		# windows
		nsis mingw-w64 \
		# linux (deb, rpm)
		fakeroot dh-make rpm \
		# code-signing
		osslsigncode && \
	mkdir /go && \
# build xar
	cd /tmp && \
	curl -sL https://github.com/downloads/mackyle/xar/xar-1.6.1.tar.gz | tar zx && \
	cd xar-1.6.1 && \
	./configure && \
	make && \
	make install && \
	ldconfig && \
# build mkbom
	cd /tmp && \
	curl -sL https://github.com/hogliux/bomutils/tarball/master | tar zx && \
	cd hogliux-bomutils-* \
	make && \
	make install && \
	ldconfig && \
# osx-cross
	git clone https://github.com/tpoechtrager/osxcross.git /osxcross && \
	cd /osxcross && \
	./tools/get_dependencies.sh && \
	curl -sL -o ./tarballs/MacOSX${OSXCROSS_SDK_VERSION}.sdk.tar.xz \
	"https://github.com/phracker/MacOSX-SDKs/releases/download/10.13/MacOSX${OSXCROSS_SDK_VERSION}.sdk.tar.xz" && \
	echo | PORTABLE=1 ./build.sh && \
	./build_compiler_rt.sh && \
	ldconfig && \
	rm -rf build tarballs/MacOSX* images && \
# cleanup
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
	apt-get clean

ENV GOPATH=/go
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/tools:/osxcross/target/bin

WORKDIR /build

CMD [ "/bin/bash" ]
