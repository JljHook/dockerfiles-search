FROM dnoliver/upm-base

# Update apt-get
RUN apt-get -y update && apt-get -y --no-install-recommends install \
    # Python 2 Build Dependencies
    python python-dev python-pip \
    # Python 3 Build Dependencies
    python3 python3-dev python3-pip \
    # Docs Build Dependencies
    python-sphinx

# Install Python Test Dependencies
RUN pip install chardet && pip3 install chardet

# Install virtualenv
RUN pip install virtualenv

# Set Workdir
WORKDIR $UPM_SRC_DIR

CMD bash
