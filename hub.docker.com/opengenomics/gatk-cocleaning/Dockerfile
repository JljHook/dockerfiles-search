
FROM broadinstitute/genomes-in-the-cloud:2.2.2-1466113830
USER root

RUN mv * /opt/
RUN ln /opt/GATK36.jar /opt/GenomeAnalysisTK.jar
ADD indel_realign.py /opt/
ADD validate_bam_header.py /opt/

WORKDIR /opt
RUN wget https://github.com/broadinstitute/picard/releases/download/1.122/picard-tools-1.122.zip
RUN unzip picard-tools-1.122.zip
RUN ln -s picard-tools-1.122 picard

RUN apt-get update && \
    apt-get install --yes python-pip
RUN pip install python-dateutil pytz

WORKDIR /tmp
VOLUME /tmp
CMD ["bash"]
