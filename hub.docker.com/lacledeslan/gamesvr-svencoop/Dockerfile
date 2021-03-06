# escape=`

FROM lacledeslan/steamcmd:linux as steamcmd-builder

# Copy cached build files (if any)
COPY ./build-cache/ /output

# Install Sven Co-op
RUN echo $'\n\nDownloading Sven Co-op via SteamCMD' &&`
        /app/steamcmd.sh `
            +login anonymous `
            +force_install_dir /output `
            +app_update 276060 validate `
            +quit;

#=======================================================================
FROM i386/debian:stretch-slim

ARG VCS_REF

HEALTHCHECK NONE

# Install dependencies
RUN apt-get update && apt-get install -y `
        libcurl3-gnutls libstdc++6 libncurses5 libz1 &&`
    apt-get clean &&`
    rm -rf /var/lib/apt/lists/*;

LABEL MAINTAINER="Laclede's LAN <contact@lacledeslan.com>" `
      org.label-schema.url="https://github.com/LacledesLAN" `
      org.label-schema.vendor="Laclede's LAN" `
      org.label-schema.vcs-ref=$VCS_REF `
      org.label-schema.name="gamesvr-svencoop" `
      org.label-schema.description="Sven Co-op in Docker" `
      org.label-schema.vcs-url="https://github.com/LacledesLAN/gamesvr-svencoop"

#Add user "SvenCoOp" (to user group "srcds")
RUN groupadd --system srcds &&`
    mkdir --parents /app/.steam/sdk32 &&`
        chgrp srcds /app &&`
    useradd `
        --home /app `
        --gid srcds `
        --system `
        SvenCoOp &&`
    chown -R SvenCoOp:srcds /app;

USER SvenCoOp

RUN ln -s /app/bin/steamclient.so /app/.steam/sdk32/steamclient.so

# Copy in from SteamCMD Builder
COPY --from=steamcmd-builder /output /app

WORKDIR /app

ONBUILD USER root
