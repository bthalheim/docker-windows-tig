# escape=`

ARG BASE=mcr.microsoft.com/windows/servercore
ARG TELEGRAF_VER="1.12.2"
ARG WINDOWS_VER="1903"

# Installer Image
FROM $BASE:$WINDOWS_VER

LABEL maintainer="https://github.com/bthalheim"
LABEL description="Telegraf running on windows servercore."

ARG TELEGRAF_VER

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

COPY net-snmp.zip c:/net-snmp.zip

RUN Invoke-WebRequest -OutFile telegraf.zip "https://dl.influxdata.com/telegraf/releases/telegraf-$($env:TELEGRAF_VER)_windows_amd64.zip"; `
    Expand-Archive telegraf.zip -DestinationPath telegraf-temp; `
    Copy-Item -Path "c:/telegraf-temp/telegraf" -Destination "c:/telegraf" -Recurse; `
    Remove-Item -Path "c:/telegraf-temp" -recurse -force; `
    Remove-Item -Path "c:/telegraf.zip" -force;


RUN expand-Archive net-snmp.zip -DestinationPath net-snmp; `
    Remove-Item -Path "c:/net-snmp.zip" -force;

SHELL ["cmd", "/S", "/C"]

USER ContainerAdministrator
RUN setx PATH "%path%;c:\net-snmp\bin" /M
RUN setx SNMPSHAREPATH "c:\net-snmp\share" /M
USER ContainerUser

WORKDIR telegraf

ENTRYPOINT ["telegraf.exe", "--console"]
