FROM golang:1.12.3 as builder
LABEL app="rivet" by="ennoo"
ENV REPO=$GOPATH/src/github.com/ennoo/rivet
WORKDIR $REPO
RUN git clone https://github.com/ennoo/rivet.git ../rivet && \
 go build -o $REPO/shunt/runner/shunt $REPO/shunt/runner/shunt.go
FROM centos:7
WORKDIR /root/
COPY --from=builder /go/src/github.com/ennoo/rivet/shunt/runner/shunt .
EXPOSE 19219
CMD ./shunt

# https://microbadger.com/labels
LABEL io.github.ennoo.name="Rivet Shunt Image" \
      io.github.ennoo.description="Load-Balance developed with golang" \
      io.github.ennoo.url="https://github.com/ennoo/rivet" \
      io.github.ennoo.license="Apache License 2.0" \
      io.github.ennoo.docker.dockerfile="/shunt/runner/Dockerfile" \
      io.github.ennoo.vcs-type="Git" \
      io.github.ennoo.vcs-url="https://github.com/ennoo/rivet.git" \
      io.github.ennoo.vendor="ENNOO"