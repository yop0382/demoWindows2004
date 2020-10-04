FROM yop038/windows-openjdk:11.0.8-jre-nanoserver-2004


RUN mkdir geoserver
WORKDIR geoserver

RUN curl -L https://freefr.dl.sourceforge.net/project/geoserver/GeoServer/2.18.0/geoserver-2.18.0-bin.zip -o geoserver-2.18.0-bin.zip & \
	tar xf geoserver-2.18.0-bin.zip & \
	del geoserver-2.18.0-bin.zip ;

WORKDIR bin

EXPOSE 8080

CMD C:\geoserver\bin\startup.bat