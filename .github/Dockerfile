FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS builder

WORKDIR /build
COPY TSLib ./TSLib
COPY TS3ABotUnitTests ./TS3ABotUnitTests
COPY TS3AudioBot ./TS3AudioBot
COPY TS3AudioBot.sln TS3AudioBot.ruleset Directory.Build.targets ./

RUN dotnet publish --framework netcoreapp2.2 --configuration Release TS3AudioBot.sln
RUN ls /build/TS3AudioBot/bin/Release/netcoreapp2.2/

FROM mcr.microsoft.com/dotnet/core/runtime:2.2

WORKDIR /data
RUN apt update && apt install -y libopus-dev ffmpeg

COPY --from=builder /build/TS3AudioBot/bin/Release/netcoreapp2.2/publish .

RUN ls -al  && ls -al lib && ls -al lib/x64

WORKDIR /data/config
ENTRYPOINT ["dotnet", "/data/TS3AudioBot.dll", "--non-interactive"]
