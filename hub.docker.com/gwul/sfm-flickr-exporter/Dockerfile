FROM gwul/sfm-base@sha256:e68cb98bdc9dc23bbed734f3e507a0ffb866b007dffea038b6af8d88a62150e6
MAINTAINER Social Feed Manager <sfm@gwu.edu>

ADD . /opt/sfm-flickr-harvester/
WORKDIR /opt/sfm-flickr-harvester
RUN pip install -r requirements/common.txt -r requirements/release.txt

ADD docker/exporter/invoke.sh /opt/sfm-setup/
RUN chmod +x /opt/sfm-setup/invoke.sh

CMD ["/opt/sfm-setup/invoke.sh"]
