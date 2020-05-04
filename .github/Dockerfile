FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS builder

WORKDIR /build
COPY . ./

RUN dotnet build --framework netcoreapp2.2 --configuration Release TS3AudioBot.sln

FROM mcr.microsoft.com/dotnet/core/runtime:3.1
WORKDIR /data
COPY --from=builder /build/TS3AudioBot/bin/Release/netcoreapp2.2/TS3AudioBot.dll .

WORKDIR /data/config
ENTRYPOINT ["dotnet", "/data/TS3AudioBot.dll"]
