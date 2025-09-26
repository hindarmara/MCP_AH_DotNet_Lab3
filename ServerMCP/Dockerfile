# Use the official .NET 9 runtime as base image
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 5205
EXPOSE 7224

# Use the .NET 9 SDK for building
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src
COPY ["ServerMCP.csproj", "."]
RUN dotnet restore "./ServerMCP.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "ServerMCP.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ServerMCP.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Copy the SQLite database
COPY beverages.sqlite .

ENTRYPOINT ["dotnet", "ServerMCP.dll"]