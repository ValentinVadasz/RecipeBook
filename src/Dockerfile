#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
RUN apt-get update -yq \
	&& apt-get install curl gnupg -yq \
	&& curl -sl https://deb.nodesource.com/setup_13.x | bash \
	&& apt-get install nodejs -yq
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
RUN apt-get update -yq \
	&& apt-get install curl gnupg -yq \
	&& curl -sl https://deb.nodesource.com/setup_13.x | bash \
	&& apt-get install nodejs -yq
WORKDIR /src
COPY ["RecipeBook.WebUI/RecipeBook.WebUI.csproj", "RecipeBook.WebUI/"]
RUN dotnet restore "RecipeBook.WebUI/RecipeBook.WebUI.csproj"
COPY . .
WORKDIR "/src/RecipeBook.WebUI"
RUN dotnet build "RecipeBook.WebUI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "RecipeBook.WebUI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RecipeBook.WebUI.dll"]