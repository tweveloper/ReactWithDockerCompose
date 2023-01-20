FROM mcr.microsoft.com/dotnet/sdk:7.0 AS dotnet-build
WORKDIR /app

# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore "react-application.csproj"
RUN dotnet build "react-application.csproj" -c Release -o /app/build

FROM dotnet-build AS dotnet-publish
# Build and publish a release
RUN dotnet publish "react-application.csproj" -c Release -o /app/publish

FROM node AS node-builder
WORKDIR /node
COPY ./ClientApp /node
RUN npm install
RUN npm run build

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS final
WORKDIR /app
RUN mkdir /app/wwwroot
COPY --from=dotnet-publish /app/publish .
COPY --from=node-builder /node/build ./wwwroot
ENTRYPOINT ["dotnet", "react-application.dll"]