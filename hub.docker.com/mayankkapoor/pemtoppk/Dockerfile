FROM python:3.5-slim

MAINTAINER mayankkapoormail@gmail.com, dineshkumar13506@gmail.com
# Update aptitude with new repo
ENV LC_ALL=C

# Create app directory
WORKDIR /usr/src/app

# Install required software
RUN    apt-get update \
    && apt-get install -y putty-tools
RUN    pip install pathlib \
    && pip install Flask \
    && pip install flask_json

# Bundle app source code
COPY app.py .

EXPOSE 5000
ENTRYPOINT ["python", "/usr/src/app/app.py"]
