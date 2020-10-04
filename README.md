_All Windows images are build for 2004 Windows Kernel :) for that i only use Nanoserver2004 with limited layers_

# Quick Start

*   Rabbitmq 3.28.9 Erlang 22.3  
    `docker build --tag rabbitmq --build-arg WIN_VER=2004 .`  
    `docker run --isolation-process -d rabbitmq`
*   PostgreSQL EDB 12.4-1 PostGis 3.0.2  
    `docker build --tag postgres-12.4-1-postgis --build-arg WIN_VER=2004 --build-arg EDB_VER=12.4-1 .`  
    `docker run --isolation-process -d postgres-12.4-1-postgis`
*   Geoserver 2.18.0  
    `docker build --tag geoserver .`  
    `docker run --isolation-process -d geoserver`

## _Use with docker-compose (v1.27.4)_

**Actually we can't deploy mixed container with v1.27 due to regression (was ok on 1.24) but will be implemented soon ...**

```
docker-compose.exe -f .\docker-compose-windows.yml up -d
SWITCH TO LINUX
docker-compose.exe -f .\docker-compose-linux.yml up -d
```
