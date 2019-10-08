# escape=`

ARG BASE=mcr.microsoft.com/windows/servercore
ARG TELEGRAF_VER="1.12.2"
ARG WINDOWS_VER="1903"


# Installer Image
#FROM $INSTALLER:$WINDOWS_VER AS installer
FROM $BASE:$WINDOWS_VER

LABEL maintainer="https://github.com/bthalheim"
LABEL description="Telegraf running on windows servercore."

ARG TELEGRAF_VER

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Invoke-WebRequest -OutFile telegraf.zip "https://dl.influxdata.com/telegraf/releases/telegraf-$($env:TELEGRAF_VER)_windows_amd64.zip"; `
    Expand-Archive telegraf.zip -DestinationPath telegraf;

RUN Invoke-WebRequest -OutFile net-snmp.zip "http://pkg.bthal.com/net-snmp.zip"; `
    Expand-Archive net-snmp.zip -DestinationPath net-snmp;

RUN Invoke-WebRequest 'https://aka.ms/vs/16/release/vc_redist.x64.exe' -OutFile 'vc_redist.x64.exe'; `
    Start-Process '.\vc_redist.x64.exe' '/install /passive /norestart' -Wait; 


# Nano Server Image
#FROM $REPO:$WINDOWS_VER
#ARG TELEGRAF_VER

SHELL ["cmd", "/S", "/C"]

#COPY --from=installer ["c:/telegraf/telegraf/","c:/telegraf"]
#COPY --from=installer ["c:/net-snmp/net-snmp/","c:/net-snmp"]

#COPY --from=installer C:\windows\system32\msvcp140.dll C:\windows\system32
#COPY --from=installer C:\windows\system32\vcruntime140.dll C:\windows\system32

USER ContainerAdministrator
RUN setx PATH "%path%;c:\net-snmp\bin" /M
RUN setx SNMPSHAREPATH "c:\net-snmp\share" /M
USER ContainerUser

WORKDIR telegraf

ENTRYPOINT ["telegraf.exe", "--console"]
