#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Echarnus.Web/Server/Echarnus.Web.Server.csproj", "Echarnus.Web/Server/"]
COPY ["Echarnus.Web/Client/Echarnus.Web.Client.csproj", "Echarnus.Web/Client/"]
COPY ["Echarnus.Web/Shared/Echarnus.Web.Shared.csproj", "Echarnus.Web/Shared/"]
RUN dotnet restore "Echarnus.Web/Server/Echarnus.Web.Server.csproj"
COPY . .
WORKDIR "/src/Echarnus.Web/Server"
RUN dotnet build "Echarnus.Web.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Echarnus.Web.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Echarnus.Web.Server.dll"]