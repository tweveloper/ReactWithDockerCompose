FROM mcr.microsoft.com/dotnet/sdk:7.0 AS dotnet-build
WORKDIR /src/react-application

# Copy everything
COPY . /src/react-application
# Restore as distinct layers
RUN dotnet restore "react-application.csproj"
RUN dotnet build "react-application.csproj" -c Release -o /app/build

FROM dotnet-build AS dotnet-publish
# Build and publish a release
RUN dotnet publish "react-application.csproj" -c Release -o /app/publish

FROM node AS node-uselagoon/node-10-builder
WORKDIR /node
COPY ./ClientApp /node
RUN npm install
RUN npm build

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS final
WORKDIR /app
RUN mkdir /app/wwwroot
COPY --from=dotnet-publish /app/publish .
COPY --from=node-builder /node/builde ./wwwroot
ENTRYPOINT ["dotnet", "react-application.dll"]