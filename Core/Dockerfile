FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
#COPY ["Core_Web/Core_Web.csproj", "Core_Web/"]
COPY . Core/
#COPY . Core_ClassLib/

# Core_ClassLib
RUN dotnet restore "Core/Core_ClassLib/Core_ClassLib.csproj"
WORKDIR "/src/Core/Core_ClassLib"
RUN dotnet build "Core_ClassLib.csproj" -c Release -o /app/build

# Core_Web
WORKDIR /src
RUN dotnet restore "Core/Core_Web/Core_Web.csproj"
COPY . .
WORKDIR "/src/Core/Core_Web"
RUN dotnet build "Core_Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Core_Web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Core_Web.dll"]