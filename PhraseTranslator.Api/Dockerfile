#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-nanoserver-1903 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-nanoserver-1903 AS build
WORKDIR /src
COPY ["PhraseTranslator.Api/PhraseTranslator.Api.csproj", "PhraseTranslator.Api/"]
RUN dotnet restore "PhraseTranslator.Api/PhraseTranslator.Api.csproj"
COPY . .
WORKDIR "/src/PhraseTranslator.Api"
RUN dotnet build "PhraseTranslator.Api.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "PhraseTranslator.Api.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "PhraseTranslator.Api.dll"]
