#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ECommerce.WebUI/ECommerce.WebUI.csproj", "ECommerce.WebUI/"]
COPY ["App.Business/App.Business.csproj", "App.Business/"]
COPY ["App.DataAccess/App.DataAccess.csproj", "App.DataAccess/"]
COPY ["App.Entities/App.Entities.csproj", "App.Entities/"]
COPY ["App.Core/App.Core.csproj", "App.Core/"]
RUN dotnet restore "./ECommerce.WebUI/ECommerce.WebUI.csproj"
COPY . .
WORKDIR "/src/ECommerce.WebUI"
RUN dotnet build "./ECommerce.WebUI.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ECommerce.WebUI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ECommerce.WebUI.dll"]