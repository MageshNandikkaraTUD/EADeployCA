# Base image for runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build image
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project file and restore
COPY . ./
RUN dotnet restore /src/FrontEnd/FrontEnd.csproj

# Build the application with the Release configuration
RUN dotnet build "./FrontEnd/FrontEnd.csproj" --configuration Release -o /app/build

# Publish to /app/publish
FROM build AS publish
RUN dotnet publish "./FrontEnd/FrontEnd.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Final stage for runtime
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FrontEnd.dll"]