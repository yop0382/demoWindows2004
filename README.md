# Quick Start

*   Rabbitmq 3.28.9 Erlang 22.3 Nanoserver 2004  
    `docker build --tag rabbitmq --build-arg WIN_VER=2004 .`  
    `docker run --isolation-process -d rabbitmq`
*   PostgreSQL EDB 12.4-1 PostGis 3.0.2  
    `docker build --tag postgres-12.4-1-postgis --build-arg WIN_VER=2004 --build-arg EDB_VER=12.4-1 .`  
    `docker run --isolation-process -d postgres-12.4-1-postgis`
*   Geoserver 2.18.0  
    `docker build --tag geoserver .`  
    `docker run --isolation-process -d geoserver`
