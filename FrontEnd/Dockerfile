# Use the official .NET SDK image to build the project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

# Set the working directory inside the container
WORKDIR /src

# Copy the entire project directory
COPY . ./

# Set the correct directory
RUN cd FrontEnd

# Restore dependencies specified in the project
RUN dotnet restore FrontEnd.csproj

# Build the application with the Release configuration
RUN dotnet build --configuration Release -o /app/build

# Publish the application (optional, if publishing is required)
RUN dotnet publish --configuration Release -o /app/publish

# Use the official .NET runtime image to run the application
FROM mcr.microsoft.com/dotnet/runtime:8.0 AS runtime

# Set the working directory for the runtime
WORKDIR /app

# Copy the published application from the build stage
COPY --from=build /app/publish ./

# Specify the command to run the application
ENTRYPOINT ["dotnet", "FrontEnd.dll"]
