FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
# Add metadata to the image to describe which port the container is listening on at runtime.
EXPOSE 80

WORKDIR /backend
# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /backend
COPY --from=build-env /backend/out .
ENTRYPOINT ["dotnet", "backend.dll"]
