FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BugTracker.WebApplication/BugTracker.WebApplication.csproj", "BugTracker.WebApplication/"]
COPY ["BugTracker.DAL/BugTracker.DAL.csproj", "BugTracker.DAL/"]
COPY ["BugTracker.Domain/BugTracker.Domain.csproj", "BugTracker.Domain/"]
RUN dotnet restore "./BugTracker.WebApplication/BugTracker.WebApplication.csproj"
COPY . .
WORKDIR "/src/BugTracker.WebApplication"
RUN dotnet build "./BugTracker.WebApplication.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./BugTracker.WebApplication.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BugTracker.WebApplication.dll"]