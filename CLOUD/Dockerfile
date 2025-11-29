# Base layer (runtime)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

# Build layer
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["CLOUD/CLOUD.csproj", "CLOUD/"]
RUN dotnet restore "CLOUD/CLOUD.csproj"

COPY . .
WORKDIR "/src/CLOUD"
RUN dotnet build "CLOUD.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish layer
FROM build AS publish
RUN dotnet publish "CLOUD.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app

# Copy published app
COPY --from=publish /app/publish .

# Cloud Run expects app to listen on PORT env var
ENV ASPNETCORE_URLS=http://0.0.0.0:8080

ENTRYPOINT ["dotnet", "CLOUD.dll"]
