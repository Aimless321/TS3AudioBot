FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS builder

WORKDIR /build
COPY TSLib ./TSLib
COPY TS3ABotUnitTests ./TS3ABotUnitTests
COPY TS3AudioBot ./TS3AudioBot
COPY TS3AudioBot.sln ./

RUN nuget restore
RUN dotnet publish --framework netcoreapp2.2 --configuration Release TS3AudioBot.sln

FROM mcr.microsoft.com/dotnet/core/runtime:2.2
WORKDIR /data

COPY --from=builder /build/TS3AudioBot/bin/Release/netcoreapp2.2/publish .

RUN apt update \
	&& apt install -y libopus-dev ffmpeg python3 axel cron \
	&& update-alternatives --install /usr/bin/python python /usr/bin/python3 99 \
	&& curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl.real && chmod a+rx /usr/local/bin/youtube-dl.real \
	&& curl -L https://gist.githubusercontent.com/Aimless321/9499c5631d80b33b1cfc8dc7415b7364/raw/e74f04bbf9e9d3423a0704289da68915adf8c18f/youtube-dl -o /usr/local/bin/youtube-dl \
	&& chmod a+rx /usr/local/bin/youtube-dl

RUN echo "56 0 * * * /usr/local/bin/youtube-dl -U > /dev/null 2> /dev/null" > cron.txt \
	&& echo "# This extra line makes it a valid cron" >> cron.txt \
	&& crontab cron.txt

WORKDIR /data/config
ENTRYPOINT ["dotnet", "/data/TS3AudioBot.dll", "--non-interactive"]
