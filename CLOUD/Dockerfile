# Base layer (runtime)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

# Build layer
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["CLOUD.csproj", "./"]
RUN dotnet restore "CLOUD.csproj"

COPY . .
RUN dotnet build "CLOUD.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish layer
FROM build AS publish
RUN dotnet publish "CLOUD.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENV ASPNETCORE_URLS=http://0.0.0.0:8080

ENTRYPOINT ["dotnet", "CLOUD.dll"]
