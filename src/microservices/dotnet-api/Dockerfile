FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /app

# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore "dotnet-api.csproj"
RUN dotnet build "dotnet-api.csproj" -c Release -o /app/build
# Build and publish a release
RUN dotnet publish "dotnet-api.csproj" -c Release -o /app/publish

# Build runtime image
FROM mcr.microsoft.com/dotnet/sdk:7.0
WORKDIR /app
COPY --from=build-env /app/publish .
ENTRYPOINT ["dotnet", "dotnet-api.dll"]