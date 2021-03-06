FROM golang:1 AS build

ARG VERSION="1.9.0"
ARG CHECKSUM="fa2e78a8cc925acc486e92eb5f9795fef1ed7b61a9d38b73d5dc20a30318d346"

ADD https://github.com/kubernetes/kube-state-metrics/archive/v$VERSION.tar.gz /tmp/kube-state-metrics.tar.gz

RUN [ "$CHECKSUM" = "$(sha256sum /tmp/kube-state-metrics.tar.gz | awk '{print $1}')" ] && \
    tar -C /tmp -xf /tmp/kube-state-metrics.tar.gz && \
    mv /tmp/kube-state-metrics-$VERSION /tmp/kube-state-metrics

RUN cd /tmp/kube-state-metrics && \
      go install && \
    echo "nogroup:*:100:nobody" > /tmp/group && \
    echo "nobody:*:100:100:::" > /tmp/passwd


FROM scratch

COPY --from=build --chown=100:100 /go/bin/kube-state-metrics /
COPY --from=build --chown=100:100 /lib/x86_64-linux-gnu/libc.so.6 \
                                  /lib/x86_64-linux-gnu/libpthread.so.0 \
                                  /lib/x86_64-linux-gnu/
COPY --from=build --chown=100:100 /lib64/ld-linux-x86-64.so.2 /lib64/
COPY --from=build --chown=100:100 /tmp/group \
                                  /tmp/passwd \
                                  /etc/

USER 100:100
EXPOSE 8080/tcp 8081/tcp
ENTRYPOINT ["/kube-state-metrics"]
CMD ["--port=8080", "--telemetry-port=8081"]
