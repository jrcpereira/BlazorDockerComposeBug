#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["BlazorDockerComposeBug/Server/BlazorDockerComposeBug.Server.csproj", "BlazorDockerComposeBug/Server/"]
COPY ["BlazorDockerComposeBug/Client/BlazorDockerComposeBug.Client.csproj", "BlazorDockerComposeBug/Client/"]
COPY ["BlazorDockerComposeBug/Shared/BlazorDockerComposeBug.Shared.csproj", "BlazorDockerComposeBug/Shared/"]
RUN dotnet restore "BlazorDockerComposeBug/Server/BlazorDockerComposeBug.Server.csproj"
COPY . .
WORKDIR "/src/BlazorDockerComposeBug/Server"
RUN dotnet build "BlazorDockerComposeBug.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorDockerComposeBug.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorDockerComposeBug.Server.dll"]
