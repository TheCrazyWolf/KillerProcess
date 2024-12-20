﻿FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 5006

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["KillerProcess.Server/KillerProcess.Server.csproj", "KillerProcess.Server/"]
COPY ["KillerProcess.Shared/KillerProcess.Shared.csproj", "KillerProcess.Shared/"]
RUN dotnet restore "KillerProcess.Server/KillerProcess.Server.csproj"
COPY . .
WORKDIR "/src/KillerProcess.Server"
RUN dotnet build "KillerProcess.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "KillerProcess.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "KillerProcess.Server.dll"]
