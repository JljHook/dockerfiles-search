FROM widukind/docker-base

ADD . /code/

RUN pip install -r requirements/tests.txt \
    && pip install --no-deps -e .
