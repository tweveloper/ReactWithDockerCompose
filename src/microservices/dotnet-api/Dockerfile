FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /src/microservices/dotnet-api

# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore
# Build and publish a release
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /src/microservices/dotnet-api
COPY --from=build-env /src/microservices/dotnet-api/out .
ENTRYPOINT ["dotnet", "dotnet-api.dll"]