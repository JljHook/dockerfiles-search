FROM microsoft/dotnet:2.1.301-sdk AS builder
WORKDIR /source

RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs

COPY *.csproj .
RUN dotnet restore

COPY ./ ./

RUN dotnet publish "./CV.csproj" --output "./dist" --configuration Release --no-restore

FROM microsoft/dotnet:2.1.1-aspnetcore-runtime
WORKDIR /app
COPY --from=builder /source/dist .
EXPOSE 80

# Make sure the app binds to port 8080
ENV ASPNETCORE_URLS http://*:8080

ENTRYPOINT ["dotnet", "CV.dll"]