ARG CONTAINER_IMAGE=microsoft/dotnet:2.2-runtime
FROM alpine:3.9 as source
ARG NewRelicVersion=8.15.455.0
ARG NewRelicUrl=https://download.newrelic.com/dot_net_agent/previous_releases/${NewRelicVersion}/newrelic-netcore20-agent_${NewRelicVersion}_amd64.tar.gz
WORKDIR /newrelic
RUN wget "${NewRelicUrl}" -O newrelic.tar.gz && gzip -dc newrelic.tar.gz | tar -xf - && rm newrelic.tar.gz

FROM $CONTAINER_IMAGE as destination
ARG BRANCH
ARG COMMIT
ARG URL
ARG VERSION

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.name="awareoss/newrelic-dotnet" \
    org.label-schema.description="dotnet core docker image containing newrelic .NET agent" \
    org.label-schema.url="https://hub.docker.com/r/awareoss/newrelic-dotnet" \
    org.label-schema.version="$VERSION" \
    org.label-schema.vcs-url=$URL \
    org.label-schema.vcs-branch=$BRANCH \
    org.label-schema.vcs-ref=$COMMIT
ENV ASPNETCORE_ENVIRONMENT=Production
ENV CORECLR_ENABLE_PROFILING=1 \
CORECLR_PROFILER={36032161-FFC0-4B61-B559-F6C5D41BAE5A} \
CORECLR_NEWRELIC_HOME=/usr/local/newrelic-netcore20-agent \
CORECLR_PROFILER_PATH=/usr/local/newrelic-netcore20-agent/libNewRelicProfiler.so \
NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true
COPY --from=source /newrelic/ /usr/local/
