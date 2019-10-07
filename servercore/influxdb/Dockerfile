# escape=`

ARG BASE=mcr.microsoft.com/windows/servercore
ARG INFLUX_VER="1.7.8"
ARG WINDOWS_VER="1903"

FROM $BASE:$WINDOWS_VER
ARG INFLUX_VER

LABEL maintainer="https://github.com/bthalheim"
LABEL description="InfluxDB running on windows servercore."


SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
                                                
RUN Invoke-WebRequest -OutFile influxdb.zip "https://dl.influxdata.com/influxdb/releases/influxdb-$($env:INFLUX_VER)_windows_amd64.zip"; `
    Expand-Archive influxdb.zip -DestinationPath influxdb-temp; `
    Copy-Item -Path "c:/influxdb-temp/influxdb-$($env:INFLUX_VER)-1" -Destination "c:/influxdb" -Recurse; `
    Remove-Item -Path "c:/influxdb-temp" -recurse -force;
    
    

EXPOSE 8086 8088

WORKDIR influxdb

ENTRYPOINT ["influxd.exe"]