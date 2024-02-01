#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Umbraco13AppOnDocker/Umbraco13AppOnDocker.csproj", "Umbraco13AppOnDocker/"]
RUN dotnet restore "./Umbraco13AppOnDocker/./Umbraco13AppOnDocker.csproj"
COPY . .
WORKDIR "/src/Umbraco13AppOnDocker"
RUN dotnet build "./Umbraco13AppOnDocker.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Umbraco13AppOnDocker.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
USER root
COPY --from=publish /app/publish .
RUN chown -R app:app /app
USER app
ENTRYPOINT ["dotnet", "Umbraco13AppOnDocker.dll"]