FROM python:3.7-slim

WORKDIR /app
COPY . ./

RUN pip install -r requirements.txt

EXPOSE 8080
CMD ["python", "-m", "ascbot"]
