FROM python:3.7-slim

WORKDIR /src

COPY . /src

RUN pip install --trusted-host pypi.python.org -r requirements.txt

CMD ["python", "app.py"]