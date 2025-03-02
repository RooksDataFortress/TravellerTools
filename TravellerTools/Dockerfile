FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .

# Use dos2unix for all files at once
RUN apt-get update && apt-get install -y dos2unix && find /src -type f -name "*.cs" -exec dos2unix {} \;

# Optional: List a problematic file to verify line endings were fixed
RUN cat /src/Grauenwolf.TravellerTools.Shared/Characters/PsionicSkillTemplate.cs | head -10 || echo "File not found"

WORKDIR /src//Grauenwolf.TravellerTools.Web
RUN dotnet restore "./Grauenwolf.TravellerTools.Web.csproj"
RUN dotnet build "./Grauenwolf.TravellerTools.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "./Grauenwolf.TravellerTools.Web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Grauenwolf.TravellerTools.Web.dll"]