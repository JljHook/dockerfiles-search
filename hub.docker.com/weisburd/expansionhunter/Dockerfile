FROM weisburd/base-image-for-str-tools:v1

MAINTAINER Ben Weisburd

ENV VERSION=v2.5.5

# install ExpansionHunter binary release from https://github.com/Illumina/ExpansionHunter/releases
RUN wget https://github.com/Illumina/ExpansionHunter/releases/download/${VERSION}/ExpansionHunter-${VERSION}-linux_x86_64.tar.gz \
    && tar xzf ExpansionHunter-${VERSION}-linux_x86_64.tar.gz \
    && rm ExpansionHunter-${VERSION}-linux_x86_64.tar.gz \
    && mv /ExpansionHunter-${VERSION}-linux_x86_64 /ExpansionHunter

WORKDIR /ExpansionHunter

COPY --from=weisburd/base-scripts-image:latest  /scripts /scripts

COPY ./pathogenic_repeats /pathogenic_repeats

# remove ENTRYPOINT because cromwell doesn't work when it's defined
#ENTRYPOINT [ "/ExpansionHunter/bin/ExpansionHunter" ]
