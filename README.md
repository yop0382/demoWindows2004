_All Windows images are build for 2004 Windows Kernel :) for that i only use Nanoserver2004 with limited layers_

[_My Docker Hub Repo_](https://hub.docker.com/u/yop038)

# Quick Start

*   Rabbitmq 3.28.9 Erlang 22.3  
    `docker build --tag rabbitmq --build-arg WIN_VER=2004 .`  
    `docker run --isolation-process -d rabbitmq`
*   PostgreSQL EDB 12.4-1 PostGis 3.0.2  
    `docker build --tag postgres-12.4-1-postgis --build-arg WIN_VER=2004 --build-arg EDB_VER=12.4-1 .`  
    `docker run --isolation-process -d postgres-12.4-1-postgis`
*   Geoserver 2.18.0 JRE 11.0.8  
    `docker build --tag geoserver .`  
    `docker run --isolation-process -d geoserver`

## _Use with docker-compose (v1.27.4)_

**Actually we can't deploy mixed container with v1.27 due to regression (was ok on 1.24) but will be implemented soon ...**

```
docker-compose.exe -f .\docker-compose-windows.yml up -d
SWITCH TO LINUX
docker-compose.exe -f .\docker-compose-linux.yml up -d
```

## LogNonReceiver

For this project, i only used .net core with VISUAL STUDIO Kubernetes Extensions and pushed my containerized code to Docker Hub.

![](https://user-images.githubusercontent.com/47157011/95011085-82f09800-062e-11eb-8e9a-23b71ce5aef9.png)

![](https://user-images.githubusercontent.com/47157011/95011090-8e43c380-062e-11eb-953a-ebc65f0279c9.png)

![](https://user-images.githubusercontent.com/47157011/95011092-93a10e00-062e-11eb-9f0b-973d093ae8de.png)
