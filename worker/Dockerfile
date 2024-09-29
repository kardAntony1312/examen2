# Build stage
FROM --platform=${BUILDPLATFORM} mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG TARGETPLATFORM
ARG TARGETARCH
RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM"

WORKDIR /source

COPY *.csproj .
RUN dotnet restore -r linux-x64

COPY . .
RUN dotnet publish -c Release -o /app -r linux-x64 --self-contained false --no-restore

# App stage
FROM mcr.microsoft.com/dotnet/runtime:7.0
WORKDIR /app

# Instalar librerías necesarias para Kafka
RUN apt-get update && apt-get install -y librdkafka-dev

COPY --from=build /app .

ENTRYPOINT ["dotnet", "Worker.dll"]
