FROM jenkins/jenkins

LABEL maintainer="Sander van Dinten <sander@vandinten.nl>"

USER root
RUN groupadd -g 994 docker && \
    usermod -G docker,jenkins jenkins && \
    apt-get update && \
    apt-get -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common && \
    curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
    add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
      $(lsb_release -cs) \
      stable" && \
    apt-get update && \
    apt-get -y install docker-ce docker-ce-cli containerd.io && \
    rm -rf /var/lib/apt/lists/*

USER jenkins