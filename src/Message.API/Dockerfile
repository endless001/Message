FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443


FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src

# It's important to keep lines from here down to "COPY . ." identical in all Dockerfiles
# to take advantage of Docker's build cache, to speed up local container builds
COPY "Message.sln" "Message.sln"

COPY "src/Message.API/Message.API.csproj" "src/Message.API/Message.API.csproj"

COPY "NuGet.config" "NuGet.config"

RUN dotnet restore "Message.sln"

COPY . .
WORKDIR /src/src/Message.API
RUN dotnet publish --no-restore -c Release -o /app
 

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Message.API.dll"]