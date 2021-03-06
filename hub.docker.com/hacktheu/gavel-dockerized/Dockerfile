FROM python:3.7-alpine

WORKDIR /opt/gavel/

ADD https://github.com/hacktheu-org/gavel/archive/v1.6.0.tar.gz /tmp/src.tar.gz

RUN tar -xzf /tmp/src.tar.gz -C /opt/gavel --strip-components 1

RUN ls

RUN set -ex \
    # some packages have runtime dependencies on system libraries
    && apk add --no-cache \
        gettext \
        # psycopg2
        postgresql-libs \
        # Pillow
        jpeg \
        zlib \
        freetype \
        lcms2 \
        libpng \
        libwebp \
        openjpeg \
        tiff \
        # numpy/scipy
        openblas

RUN set -ex \
    # some packages need to be compiled from source and have build-time dependencies
    && apk add --no-cache --virtual .build-deps \
        gcc \
        build-base \
        musl-dev \
        # psycopg2
        postgresql-dev \
        # Pillow
        jpeg-dev \
        zlib-dev \
        freetype-dev \
        lcms2-dev \
        libpng-dev \
        libwebp-dev \
        openjpeg-dev \
        tiff-dev \
        # scipy
        openblas-dev \
    && pip install --no-cache-dir -r requirements.txt \
    && apk del .build-deps

EXPOSE 80
ENV PORT=80
CMD ["sh", "-c", "python initialize.py && gunicorn -b :${PORT} -w 3 gavel:app"]

COPY . .
