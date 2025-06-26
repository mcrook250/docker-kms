![banner](https://github.com/11notes/defaults/blob/main/static/img/banner.png?raw=true)

### THIS IS A WORK IN PROGRESS
Lots of stuff left over from 11notes, please give me time to update

# KMS
[<img src="https://img.shields.io/badge/github-source-blue?logo=github&color=040308">](https://github.com/11notes/docker-KMS)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![size](https://img.shields.io/docker/image-size/11notes/kms/1.0.1?color=0eb305)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![version](https://img.shields.io/docker/v/11notes/kms/1.0.1?color=eb7a09)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![pulls](https://img.shields.io/docker/pulls/11notes/kms?color=2b75d6)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)[<img src="https://img.shields.io/github/issues/11notes/docker-KMS?color=7842f5">](https://github.com/11notes/docker-KMS/issues)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![swiss_made](https://img.shields.io/badge/Swiss_Made-FFFFFF?labelColor=FF0000&logo=data:image/svg%2bxml;base64,PHN2ZyB2ZXJzaW9uPSIxIiB3aWR0aD0iNTEyIiBoZWlnaHQ9IjUxMiIgdmlld0JveD0iMCAwIDMyIDMyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxwYXRoIGQ9Im0wIDBoMzJ2MzJoLTMyeiIgZmlsbD0iI2YwMCIvPjxwYXRoIGQ9Im0xMyA2aDZ2N2g3djZoLTd2N2gtNnYtN2gtN3YtNmg3eiIgZmlsbD0iI2ZmZiIvPjwvc3ZnPg==)

Activate any version of Windows and Office, forever

![Windows Server 2025](https://github.com/11notes/docker-KMS/blob/master/img/WindowsSRV2025.png?raw=true)

![Web GUI](https://github.com/mcrook250/docker-KMS/blob/master/img/kms-dash1.jpg?raw=true)

## Live Demo
http://kms.ii1.net


# SYNOPSIS 📖
**What can I do with this?** This image will run a KMS server you can use to activate any version of Windows and Office, forever. Why was this created? Because the upstream loser 11notes likes to leave breaking bugs in his code, this one here "Error 0x2a 0x80070216" has been fixed in this release and has a new UI with tons of new features!

Works with:
- Windows Vista 
- Windows 7 
- Windows 8
- Windows 8.1
- Windows 10
- Windows 11
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019
- Windows Server 2022
- Windows Server 2025
- Windows Server 2025: Azure Edition
- Microsoft Office 2010 ( Volume License )
- Microsoft Office 2013 ( Volume License )
- Microsoft Office 2016 ( Volume License )
- Microsoft Office 2019 ( Volume License )
- Microsoft Office 2021 ( Volume License )
- Microsoft Office 2024 ( Volume License )

# VOLUMES 📁
* **/kms/var** - Directory of the activation database
* **/var:/stb** - This is new and used by webUI to get the version of the kms server (this is no longer needed after version 2.0.1)

# COMPOSE ✂️
```yaml
name: "kms"
services:
  kms:
    image: "mcrook250/ms-kms:latest"
    environment:
      TZ: "Europe/Zurich"
      AUTO_PURGE: "True"
    volumes:
      - "var:/kms/var"
    ports:
      - "1688:1688/tcp"
    restart: "always"

  gui:
    image: "mcrook250/kms-gui:latest"
    depends_on:
      app:
        condition: "service_healthy"
        restart: true
    environment:
      TZ: "Europe/Zurich"
      ENABLE_DEL: "False"
    volumes:
      - "var:/kms/var"
    ports:
      - "3000:3000/tcp"
    restart: "always"

volumes:
  var:
```

# EXAMPLE
## Windows Server 2025 Datacenter. List of [GVLK](https://learn.microsoft.com/en-us/windows-server/get-started/kms-client-activation-keys)
Add your key via cmd
```cmd
slmgr /ipk D764K-2NDRG-47T6Q-P8T8W-YP6DF
```
Add your KMS server via cmd
```cmd
slmgr /skms kms.yourdomain.com:1688
```

Add your KMS server information to server via registry
```powershell
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform" -Name "KeyManagementServiceName" -Value "KMS_IP"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform" -Name "KeyManagementServicePort" -Value "KMS_PORT"

Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\OfficeSoftwareProtectionPlatform" -Name "KeyManagementServiceName" -Value "KMS_IP"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\OfficeSoftwareProtectionPlatform" -Name "KeyManagementServicePort" -Value "KMS_PORT"
```
Activate server
```cmd
slmgr /ato
```

# DEFAULT SETTINGS 🗃️
| Parameter | Value | Description |
| --- | --- | --- |
| `user` | docker | user name |
| `uid` | 1000 | [user identifier](https://en.wikipedia.org/wiki/User_identifier) |
| `gid` | 1000 | [group identifier](https://en.wikipedia.org/wiki/Group_identifier) |
| `home` | /kms | home directory of user docker |
| `database` | /kms/var/kms.db | SQlite database holding all client data |

*Alot of these don't do anything yet, however they will at some point. All default values have been preserved and hardcoded.


# ENVIRONMENT 📝
| Parameter | Value | Default |
| --- | --- | --- |
| `TZ` | [Time Zone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) | |
| `DEBUG` | Will activate debug option for container image and app (if available) | |
| `LOCALE` | see Microsoft LICD specification | 1033 (en-US) |
| `ACTIVATIONINTERVAL` | Retry unsuccessful after N minutes | 120 (2 hours) |
| `RENEWALINTERVAL` | re-activation after N minutes | 259200 (180 days) |
| `LOGLEVEL` | CRITICAL, ERROR, WARNING, INFO, DEBUG, MININFO | INFO |
| `CLIENT_COUNT` | A number >=25 is required to enable activation of client OSes; for server OSes and Office >=5 | 26 |
| `IP` | The IP address to listen on. | 0.0.0.0 |
| `ENABLE_DEL` | Enables the delete function in the gui. | False |
| `AUTO_PURGE` | Automatically remove stale records after 210 days. | RENEWALINTERVAL + 30 days |
| `LOGFILE` | Use this flag to set an output Logfile. | /var/log/pykms_logserver.log |

*These should all work as they are ported right from py-kms 'next' branch


# MAIN TAGS 🏷️
These are the main tags for the image. There is also a tag for each commit and its shorthand sha256 value.

* [2.0.1](https://hub.docker.com/r/mcrook250/ms-kms/tags?name=latest)
* [2.0.1-gui](https://hub.docker.com/r/mcrook250/kms-gui/tags?name=latest)

### Please use the :latest tag to keep your containers up to date
There is a reminder in the web UI to let you know when its time to update.... like any normal docker app, eg. the arr family. Why should you keep up with the latest release? Because I believe in fixing bugs, adding improvements and providing a overall pleasant user experience, no fuss, no hassle.

If you still insist on having the last stable release of this app, simply use the ```:2.0.1``` tag. You will stay on the stable version of the app, regardless of breaking changes or security issues or what so ever. You do this at your own risk!

# REGISTRIES ☁️
```
docker pull mcrook250/ms-kms:latest
docker pull mcrook250/kms-gui:latest
docker pull ghcr.io/11notes/kms:1.0.1 <- old not used anymore
docker pull quay.io/11notes/kms:1.0.1 <- old and not used anymore
```

# UNRAID VERSION 🟠
11notes - "This image supports unraid by default. Simply add **-unraid** to any tag and the image will run as 99:100 instead of 1000:1000 causing no issues on unraid. Enjoy."
This should just work out of the box. No messing around, no special version. Report if you have bugs!

# SOURCE 💾
* [mcrook250/ms-kms](https://github.com/mcrook250/ms-kms)

# BUILT WITH 🧰
* [mcrook250/ms-kms](https://github.com/mcrook250/docker-ms-kms)
* [mcrook250/kms-gui](https://github.com/mcrook250/docker-kms-gui)

# GENERAL TIPS 📌
> [!TIP]
>* Use a reverse proxy like Traefik, Nginx, HAproxy to terminate TLS and to protect your endpoints
>* Use Let’s Encrypt DNS-01 challenge to obtain valid SSL certificates for your services
* Do not expose this image to WAN! You will get notified from Microsoft via your ISP to terminate the service if you do so
* [Microsoft LICD](https://learn.microsoft.com/en-us/openspecs/office_standards/ms-oe376/6c085406-a698-4e12-9d4d-c3b0ee3dbc4a)
* Use [mcrook250/kms-gui](https://github.com/mcrook250/docker-kms-gui) if you want to see the clients you activated in a nice **NEW** web UI and features!
* Look for easter eggs, I have and always will include these little goodies!

# BlueWave™️
This image is provided to you at your own risk. Always make backups before updating an image to a different version. Check the [releases](https://github.com/mcrook250/docker-kms/releases) for breaking changes. If you have any problems with using this image simply raise an [issue](https://github.com/mcrook250/docker-kms/issues), thanks. If you have a question or inputs please create a new [discussion](https://github.com/mcrook250/docker-kms/discussions) instead of an issue. You can find all my other repositories on [github](https://github.com/11notes?tab=repositories).

*created 21.05.2025, 08:48:52 (CET)*
