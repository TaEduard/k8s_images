ARG BASE_IMAGE=microsoft/windowsservercore:1803
FROM $BASE_IMAGE
EXPOSE 80
ENV chocolateyUseWindowsCompression false
RUN powershell -Command \
    Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')); \
    choco feature disable --name showDownloadProgress
RUN powershell -Command \
    choco install apache-httpd -y; \
    choco install php -y --params '"/ThreadSafe "'
RUN powershell -Command \
    $oldpath = (Get-ItemProperty -Path ‘Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment’ -Name PATH).path; \
    $newpath = “$oldpath;c:\tools\php72\;c:\Users\ContainerAdministrator\AppData\Roaming\Apache24\bin\”; \
    Set-ItemProperty -Path ‘Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment’ -Name PATH -Value $newPath
RUN powershell -Command \
    cd c:/Users/ContainerAdministrator/AppData/Roaming/Apache24/htdocs/ ; \
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12 ; \
    Invoke-WebRequest -Method Get -Uri https://github.com/nrk/predis/archive/v1.1.1.zip -OutFile Predis.zip; \
    Expand-Archive Predis.zip .; \
    Remove-Item Predis.zip -Force; \
    mv predis-1.1.1 Predis
COPY php.ini tools/php72/php.ini
COPY httpd.conf /Users/ContainerAdministrator/AppData/Roaming/Apache24/conf/httpd.conf
ADD guestbook.php /Users/ContainerAdministrator/AppData/Roaming/Apache24/htdocs/guestbook.php
ADD controllers.js /Users/ContainerAdministrator/AppData/Roaming/Apache24/htdocs/controllers.js
ADD index.html /Users/ContainerAdministrator/AppData/Roaming/Apache24/htdocs/index.html
COPY run.ps1 run.ps1
CMD .\run.ps1
