FROM golang:1.8.5-jessie as builder
# install xz
RUN apt-get update && apt-get install -y \
    xz-utils \
    && rm -rf /var/lib/apt/lists/*
# install UPX
ADD https://github.com/upx/upx/releases/download/v3.94/upx-3.94-amd64_linux.tar.xz /usr/local
RUN xz -d -c /usr/local/upx-3.94-amd64_linux.tar.xz | \
    tar -xOf - upx-3.94-amd64_linux/upx > /bin/upx && \
    chmod a+x /bin/upx
# install dep
RUN go get github.com/golang/dep/cmd/dep

ENV GOPATH /go
# create a working directory
WORKDIR /go/src/github.com/yale-mgt-656-fall-2018/server-hw-grading
# add Gopkg.toml and Gopkg.lock
COPY Gopkg.toml Gopkg.toml
COPY Gopkg.lock Gopkg.lock
# install packages
RUN dep ensure --vendor-only
# add source code
COPY main.go main.go
COPY questions questions

# build the source
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main 
# strip and compress the binary
RUN strip --strip-unneeded main
RUN upx main

# use scratch (base for a docker image)
FROM scratch
# set working directory
WORKDIR /root
# copy the binary from builder
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /go/src/github.com/yale-mgt-656-fall-2018/server-hw-grading/main .
# run the binary
ENTRYPOINT [ "./main" ]