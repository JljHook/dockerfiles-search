FROM python:3.6-alpine

RUN apk add --update postgresql-libs && \
    apk add --udpate --virtual .build-deps gcc musl-dev postgresql-dev tzdata && \
    cp /usr/share/zoneinfo/Europe/Paris /etc/localtime && \
    echo "Europe/Paris" >  /etc/timezone

WORKDIR /app
EXPOSE 8000
VOLUME /app/staticfiles

RUN pip3 install pipenv
COPY Pipfile Pipfile.lock ./
RUN pipenv install --deploy

RUN apk del .build-deps

COPY . ./

CMD ["sh", "bash/run-prod.sh"]

HEALTHCHECK --interval=10s --timeout=10s CMD ["pipenv", "run", "python", "healthcheck.py"]

ENV DATABASE_URL postgres://postgresql:postgresql@db:5432/manuels
ENV SECRET_KEY ''
ENV MAILGUN_ACCESS_KEY ''
ENV MAILGUN_SERVER_NAME ''
ENV DJANGO_ENV ''
ENV ADMIN_EMAIL ''
ENV SERVER_EMAIL ''
ENV HOST ''
ENV REPLY_TO ''
ENV AUTHORIZED_EMAILS ''
ENV LIBRARIAN_EMAILS ''
