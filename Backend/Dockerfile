# Base image for final runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0-jammy AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build image
FROM mcr.microsoft.com/dotnet/sdk:8.0-jammy AS build
WORKDIR /src

# Copy project file and restore dependencies
COPY . ./
RUN dotnet restore /src/Backend/Backend.csproj

# Build the project
RUN dotnet build "./Backend/Backend.csproj" -c Release -o /app/build

# Publish to /app/publish
FROM build AS publish
RUN dotnet publish "./Backend/Backend.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Final stage (used in release/debug)
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Backend.dll"]