FROM python:3.7

RUN apt-get update && apt-get install -y python3 sudo

RUN mkdir /data

WORKDIR /data

COPY api/requirements.txt /requirements.txt

RUN pip install -r /requirements.txt

COPY api /data

EXPOSE 5000

CMD ["gunicorn", "main:app", "-b", "0.0.0.0:5000", "-w", "2"]
