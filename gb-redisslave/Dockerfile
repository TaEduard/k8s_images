ARG BASE_IMAGE=microsoft/windowsservercore:1803
FROM $BASE_IMAGE
LABEL Description="Redis" Vendor="MSOpenTech" Version="3.2"
EXPOSE 6379
WORKDIR /redis
RUN powershell -Command \
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12 ; \
Invoke-WebRequest -Method Get -Uri https://github.com/MSOpenTech/redis/releases/download/win-3.2.100/Redis-x64-3.2.100.zip -OutFile redis.zip ; \
Expand-Archive redis.zip . ; \
Remove-Item redis.zip -Force
COPY redis.conf ./redis.windows-service.conf
COPY redis.conf ./redis.windows.conf
COPY run.ps1 run.ps1
CMD .\run.ps1
