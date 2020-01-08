FROM python:3.7
ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONIOENCODING=UTF-8
RUN apt update && apt dist-upgrade -y
RUN apt install exiv2 build-essential python-all-dev libexiv2-dev libboost-python-dev libpq-dev locales tzdata netcat -y
RUN mkdir /repo
RUN mkdir /src
RUN mkdir /config
RUN mkdir /photo_directory
RUN mkdir /var/log/gunicorn
RUN git clone https://github.com/Aninstance/simple-photo-management.git /repo
RUN cp -a /repo/src/. /src
RUN cp -a /repo/config/. /config
RUN rm -rf /repo
RUN pip install -r /config/requirements.txt
WORKDIR /src
EXPOSE 8000
VOLUME ["/photo_directory"]
ENTRYPOINT [ "/config/entrypoint-dev-server.sh" ]