# escape=`

ARG BASE=mcr.microsoft.com/windows/servercore
ARG GRAFANA_VER="6.3.6"
ARG WINDOWS_VER="1903"

FROM $BASE:$WINDOWS_VER
ARG GRAFANA_VER


SHELL ["cmd", "/S", "/C"]

# Grafana Configuration And Data Paths
ENV GF_PATHS_CONFIG="c:\grafana_config\grafana.ini" `
    GF_PATHS_DATA="c:\grafana_data" `
    GF_PATHS_HOME="c:\grafana" ` 
    GF_PATHS_LOGS="c:\grafana_logs" `
    GF_PATHS_PLUGINS="c:\grafana_plugins" `
    GF_PATHS_PROVISIONING="c:\grafana_provisioning" 


# "%GF_PATHS_HOME%" `
RUN mkdir `
    "%GF_PATHS_DATA%" `   
    "%GF_PATHS_LOGS%" `
    "%GF_PATHS_PLUGINS%" 
    
    #"%GF_PATHS_PROVISIONING%\datasources" `
    #"%GF_PATHS_PROVISIONING%\dashboards" 

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]


RUN Invoke-WebRequest -OutFile grafana.zip https://dl.grafana.com/oss/release/grafana-$env:GRAFANA_VER.windows-amd64.zip; `
    Expand-Archive grafana.zip -DestinationPath grafana-temp; `
    Copy-Item -Path "c:/grafana-temp/grafana-$env:GRAFANA_VER" -Destination "c:/grafana" -Recurse; `
    Copy-Item -Path "c:/grafana-temp/grafana-$env:GRAFANA_VER/conf" -Destination "c:/grafana_config/" -Recurse; `
    Copy-Item -Path "c:/grafana-temp/grafana-$env:GRAFANA_VER/conf/provisioning" -Destination "c:/grafana_provisioning" -Recurse;


EXPOSE 3000

WORKDIR grafana/bin

ENTRYPOINT ["grafana-server.exe"]
