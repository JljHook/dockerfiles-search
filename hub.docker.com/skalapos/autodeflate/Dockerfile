FROM mcr.microsoft.com/azure-functions/python:2.0-python3.6-appservice

ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
    AzureFunctionsJobHost__Logging__Console__IsEnabled=true

COPY . /home/site/wwwroot

RUN cd /home/site/wwwroot && \
    pip install -r requirements.txt