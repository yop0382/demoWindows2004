# escape=`
ARG WIN_VER

# Setup shared variables
ARG ERLANG_VERSION=22.3
ARG RABBITMQ_VERSION=3.8.9

# Use server core to support erlang install
FROM mcr.microsoft.com/windows/servercore:${WIN_VER} as source

# Setup PowerShell as default Run Shell
SHELL ["PowerShell.exe", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue'; "]

# Environment Variables (ARGs needed to see outer scope ARGs)
ARG ERLANG_VERSION
ARG RABBITMQ_VERSION
ENV ERLANG_VERSION=$ERLANG_VERSION `
    RABBITMQ_VERSION=$RABBITMQ_VERSION

# Install Erlang OTP & RabbitMQ
RUN Write-Host -Object 'Downloading Erlang OTP' ; `
    $erlangInstaller = Join-Path -Path $env:Temp -ChildPath 'otp_win64.exe' ; `
    Invoke-WebRequest -UseBasicParsing -Uri $('http://erlang.org/download/otp_win64_{0}.exe' -f $env:ERLANG_VERSION) -OutFile $erlangInstaller ; `
    Unblock-File -Path $erlangInstaller ; `
    Write-Host -Object 'Installing Erlang OTP' ; `
    Start-Process -NoNewWindow -Wait -FilePath $erlangInstaller -ArgumentList /S, /D=c:\erlang ; `
    Write-Host -Object 'Removing Erlang OTP Installer' ; `
    Remove-Item -Path $erlangInstaller ; `
    Write-Host -Object 'Done Installing Erlang OTP' ; `
    Write-Host -Object 'Downloading RabbitMQ Zip' ; `
    $rabbitZip = Join-Path -Path $env:Temp -ChildPath 'rabbitmq.zip' ; `
    Invoke-WebRequest -UseBasicParsing -Uri $('https://github.com/rabbitmq/rabbitmq-server/releases/download/v{0}/rabbitmq-server-windows-{0}.zip' -f $env:RABBITMQ_VERSION) -OutFile $rabbitZip ; `
    Unblock-File -Path $rabbitZip ; `
    Write-Host -Object 'Extracting RabbitMQ' ; `
    Expand-Archive -Path $rabbitZip -DestinationPath c:\ ; `
    Rename-Item c:\rabbitmq_server-$env:RABBITMQ_VERSION c:\rabbitmq ; `
    Write-Host -Object 'Removing RabbitMQ Zip' ; `
    Remove-Item -Path $rabbitZip ; `
    Write-Host -Object 'Done Installing RabbitMQ'

### Install correct Visual C++ Redistributable Package
RUN Write-Host('Visual C++ 2015 Redistributable Package') ; `
    $URL2 = 'https://download.microsoft.com/download/9/3/F/93FCF1E7-E6A4-478B-96E7-D4B285925B00/vc_redist.x64.exe' ; `
    Invoke-WebRequest -Uri $URL2 -OutFile 'C:\\vcredist.exe' ; `
    Start-Process 'C:\\vcredist.exe' -Wait `
        -ArgumentList @( `
            '/install', `
            '/passive', `
            '/norestart' `
        )

RUN Write-Host('Visual C++ 2017 Redistributable Package') ; `
    $URL2 = 'https://download.visualstudio.microsoft.com/download/pr/11100230/15ccb3f02745c7b206ad10373cbca89b/VC_redist.x64.exe' ; `
    Invoke-WebRequest -Uri $URL2 -OutFile 'C:\\vcredist.exe' ; `
    Start-Process 'C:\\vcredist.exe' -Wait `
        -ArgumentList @( `
            '/install', `
            '/passive', `
            '/norestart' `
        )

RUN DIR c:\erlang\

RUN Copy-Item 'C:\\windows\\system32\\vcruntime140.dll' -Destination 'C:\\erlang\\erts-10.7\\bin\\vcruntime140.dll' ;

# Start from nano server
FROM mcr.microsoft.com/windows/nanoserver:${WIN_VER}

# Copy erlang and rabbit
COPY --from=source C:\erlang\ c:\erlang\
COPY --from=source C:\rabbitmq\ c:\rabbitmq\

# Environment Variables (ARGs needed to see outer scope ARGs)
ENV ERLANG_HOME=c:\erlang\erts-10.7 `
    RABBITMQ_HOME=c:\rabbitmq `
    RABBITMQ_BASE=c:\data `
    ERLANG_VERSION=$ERLANG_VERSION `
	HOMEDRIVE=c: `
	HOMEPATH=\Users\ContainerUser

# setup persistent folders
# VOLUME $RABBITMQ_BASE

# setup working directory
WORKDIR $RABBITMQ_HOME\sbin

# Ports
# 4369: epmd, a peer discovery service used by RabbitMQ nodes and CLI tools
# 5672: used by AMQP 0-9-1 and 1.0 clients without TLS
# 5671: used by AMQP 0-9-1 and 1.0 clients with TLS
# 25672: used by Erlang distribution for inter-node and CLI tools communication and is allocated from a dynamic range (limited to a single port by default, computed as AMQP port + 20000).
# 15672: HTTP API clients and rabbitmqadmin (only if the management plugin is enabled)
# 61613: STOMP clients without TLS (only if the STOMP plugin is enabled)
# 61614: STOMP clients with TLS (only if the STOMP plugin is enabled)
# 1883: MQTT clients without TLS, if the MQTT plugin is enabled
# 8883: MQTT clients with TLS, if the MQTT plugin is enabled
# 15674: STOMP-over-WebSockets clients (only if the Web STOMP plugin is enabled)
# 15675: MQTT-over-WebSockets clients (only if the Web MQTT plugin is enabled)
EXPOSE 4369 5672 5671 25672 15672 61613 61614 1883 8883 15674 15675

# copy blank .erlang.cooke file (should be replaced by start.cmd at startup)
#COPY .erlang.cookie c:\
ADD data.runtime\ $RABBITMQ_BASE

#VOLUME $RABBITMQ_BASE

# grant permission to .erlang.cooke
# RUN icacls c:\.erlang.cookie /grant %USERNAME%:F

# run external command when container starts to allow for additional setup
CMD c:\data\start.cmd
