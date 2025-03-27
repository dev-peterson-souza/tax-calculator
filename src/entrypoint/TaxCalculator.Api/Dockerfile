FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/entrypoint/TaxCalculator.Api/TaxCalculator.Api.csproj", "src/entrypoint/TaxCalculator.Api/"]
RUN dotnet restore "src/entrypoint/TaxCalculator.Api/TaxCalculator.Api.csproj"
COPY . .
WORKDIR "/src/src/entrypoint/TaxCalculator.Api"
RUN dotnet build "TaxCalculator.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "TaxCalculator.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TaxCalculator.Api.dll"]
