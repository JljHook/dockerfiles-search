# Build sources.
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /src
COPY . ./
WORKDIR /src/src/Recollections.Api
RUN dotnet publish -c Release -r linux-arm -o /app

# Final image.
FROM neptuo/aspnet-system-drawing:2.2-stretch-slim-arm32v7
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["./Recollections.Api"]