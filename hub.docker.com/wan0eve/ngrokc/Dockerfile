FROM ubuntu:14.04.2
MAINTAINER william0wang

# Install Root CA
RUN apt-get update
RUN apt-get install -y ca-certificates

# Install ngrok
COPY ngrok /bin/ngrok
RUN chmod 755 /bin/ngrok

CMD ngrok -log stdout -log-level INFO -proto "${proto}" --subdomain=${sub_domain} --hostname=${host_name} -config ${cfg_file} ${in_port}
