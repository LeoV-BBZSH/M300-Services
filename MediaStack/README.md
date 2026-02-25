Quelle: https://github.com/navilg/media-stack/tree/main

# Installation

## Voraussetzungen
- Debian 13 aufgesetzt
- NAS, auf welches ein NFS Mount gemacht werden kann. 


## Mount erstellen

nfs-common auf der VM installieren:

```shell
sudo apt update
sudo apt install nfs-common
```

Um den Share Temporär zu Mounten, kann der folgende Command verwendet werden:
```shell
sudo mount -t nfs [IP-NAS]:[Share-auf-dem-NAS] [mountpunkt auf der VM]
```

Beispiel:
```shell
sudo mount -t nfs 192.168.1.20:volume1/Watch /mnt/watch
```

Anschliessend kann man auf dem NAS eine Datei erstellen, und kontrollieren, ob diese in der VM angezeigt werden. Wenn dies der Fall ist, muss man den NFS Share dauerhaft Mounten, da er im aktuellen Zustand nach einem Neustart der VM nicht mehr vorhanden wäre. 

Um den Share dauerhaft zu mounten, muss man die Datei `/etc/fstab` bearbeiten, und den Mount hinzufügen. Dazu wird unten einfach eine weitere Linie ergänzt. 
```
[IP-NAS]:[Share-auf-dem-NAS] [mountpunkt auf der VM] nfs defaults,_netdev 0 0
```

Beispiel:
```
192.168.1.20:/volume1/Watch /mnt/watch nfs defaults,_netdev 0 0
```
![](Pasted%20image%2020260225220832.png)
## Docker Compose anpassen
Nun muss das Docker Compose, welches ich aus dem Quell Git Repo kopiert habe auf meine Bedürfnisse angepasst werden.  Ich werde nur auf die wichtigsten Änderungen eingehen,  da die yml Dateien zu lange sind, alles genau anzuschauen. 

Geänderte Dinge:
- **VPN Configuration für Proton VPN vorbereitet, ich werde diesen aber (zumindest in näherer Zukunft) nicht verwenden.** 
- **qbittorenrent durch Transmission ersetz**
  Der Hintergrund hinter diesem Schritt ein rechtlicher. Da ich mich in der Schweiz befinde, ist es erlaubt Filme im Internet Gratis herunterzuladen, zb. von Piratebay. Das hochladen ist allerdings auch in der Schweiz strafbar. Da die meisten Download Clients keine Begrenzung des Uploads auf 0 erlauben, muss ich einen verwenden, bei welchem dies möglich ist. Ein solcher ist Transmission. 
  Die Daten für den "Transmission" service habe ich aus dem "Transmission" [Repository](https://github.com/linuxserver/docker-transmission). 
- **Zeitzone in den verschiedenen Services angepasst**
- **Das Volume "transmission-downloads" in allen services durch den Mountpunkt ersetzt, welcher auf das NAS zeigt**



Neue docker-compose.yml

```yaml
version: "3.9"

name: media-stack

services:

  

  # To use/enable VPN, Run this compose file with --profile=vpn. Its highly recommended to use VPN.

  vpn:

    ## Read https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers for details on configuring VPN for your service provider.

    profiles: ["vpn"]

    container_name: vpn

    image: qmcgaw/gluetun:v3.41.1

    cap_add:

      - NET_ADMIN

    environment:

      - VPN_SERVICE_PROVIDER=${VPN_SERVICE_PROVIDER:-protonvpn} # Valid values: nordvpn, expressvpn, protonvpn, surfshark or custom

      - OPENVPN_USER=${OPENVPN_USER:-"xxxx@xxxxxxxxxxxxxxx.ch"}

      - OPENVPN_PASSWORD=${OPENVPN_PASSWORD:-"xxxxxxxxxxxxxxxxxxxxxxxxxxxx"}

  

      ## For list of server countries, visit https://raw.githubusercontent.com/qdm12/gluetun/master/internal/storage/servers.json

      ## When VPN_SERVICE_PROVIDER is custom. Comment the below line

      - SERVER_COUNTRIES=${SERVER_COUNTRIES:-Switzerland}

      - FREE_ONLY=on  # Valid with protonvpn only. Value willbe set "on" if using free subscription provided by protonvpn

  

      ## Enable below if VPN_SERVICE_PROVIDER=custom

      # - VPN_TYPE=openvpn # or wireguard.

  

      ## If VPN_TYPE is openvpn

      # - OPENVPN_CUSTOM_CONFIG=/gluetun/custom.conf

  

      ## If VPN_TYPE is wireguard. Replace below env variables as required.

      # - VPN_ENDPOINT_IP=1.2.3.4                                               # Replace with your wg endpoint ip or domain

      # - VPN_ENDPOINT_PORT=51820                                               # Replace with wg server port

      # - WIREGUARD_PUBLIC_KEY=wAUaJMhAq3NFutLHIdF8AN0B5WG8RndfQKLPTEDHal0=     # Replace with your wg public key

      # - WIREGUARD_PRIVATE_KEY=wOEI9rqqbDwnN8/Bpp22sVz48T71vJ4fYmFWujulwUU=    # Replace with your wg client private key

      # - WIREGUARD_PRESHARED_KEY=xOEI9rqqbDwnN8/Bpp22sVz48T71vJ4fYmFWujulwUU=  # Replaced with your wg pre-shared key

      # - WIREGUARD_ADDRESSES="10.64.222.21/32"                                 # Replace with wg address

  

    ## Enable volume if VPN_SERVICE_PROVIDER=custom and VPN_TYPE=openvpn is used

    # volumes:

      # - /yourpath/yourconfig.conf:/gluetun/config.conf:ro

  

    networks:

      - mynetwork

  

    devices:

      - /dev/net/tun:/dev/net/tun

    # Uncomment/enable below ports if VPN is used/enabled

    # ports:

    #   # qbittorrent ports

    #   - 5080:5080

    #   - 6881:6881

    #   - 6881:6881/udp

    #   # prowlarr ports

    #   - 9696:9696

    volumes:

      - gluetun-data:/gluetun

    restart: "unless-stopped"

  

  ## Default credentials of qBittorrent - Username: admin password: adminadmin ##

  ## Change password after install from UI --> Tools --> Options --> WebUI ##

 # qbittorrent:

  #  profiles: ["vpn", "no-vpn"]

   # container_name: qbittorrent

    #image: lscr.io/linuxserver/qbittorrent:5.1.4

  

    # Unomment below if vpn is enabled

    # depends_on:               # Uncomment this line if vpn is enabled

      # vpn:                   # Uncomment this line if vpn is enabled

      #   condition: service_healthy # Uncomment this line if vpn is enabled

  

    # Comment below lines if VPN is enabled

    #networks:               # Comment this line if vpn is enabled

     # - mynetwork           # Comment this line if vpn is enabled

  

    # Unomment below line if vpn is enabled

    # network_mode: service:vpn

  

    #environment:

     # - PUID=1000

     # - PGID=1000

     # - TZ=UTC

     # - WEBUI_PORT=5080

    #volumes:

    #  - qbittorrent-config:/config

    #  - torrent-downloads:/downloads

  

    ## Comment/Disable below ports if VPN is enabled

   # ports:

  #  - 5080:5080

 #   - 6881:6881

  #  - 6881:6881/udp

#    restart: "unless-stopped"

  transmission:

    profiles: ["vpn", "no-vpn"]

    image: lscr.io/linuxserver/transmission:latest

    container_name: transmission

    networks:

      - mynetwork

    environment:

      - PUID=1000

      - PGID=1000

      - TZ=Europe/Zurich

      - TRANSMISSION_WEB_HOME= #optional

      - USER=xxxxxxxxxxxxxxxxxxxxxxxx

      - PASS=xxxxxxxxxxxxxxxxxxxxxxxxxxxx

      - WHITELIST= #optional

      - PEERPORT= #optional

      - HOST_WHITELIST= #optional

    volumes:

      - transmission-config:/config

      - /mnt/watch:/downloads #optional

    ports:

      - 9091:9091

      - 51413:51413

      - 51413:51413/udp

    restart: unless-stopped

  

  radarr:

    profiles: ["vpn", "no-vpn"]

    container_name: radarr

    image: lscr.io/linuxserver/radarr:6.0.4

    networks:

      - mynetwork   # Comment this line if VPN is enabled

    ## Uncomment below lines if VPN is enabled

    #   mynetwork:

    #     ipv4_address: ${RADARR_STATIC_CONTAINER_IP} # It should be available IPv4 address in range of docker network `mynetwork` e.g. 172.20.0.2

    environment:

      - PUID=1000

      - PGID=1000

      - TZ=Europe/Zurich

    ports:

      - 7878:7878

    volumes:

      - radarr-config:/config

      - /mnt/watch/downloads:/downloads

    restart: "unless-stopped"

  

  sonarr:

    profiles: ["vpn", "no-vpn"]

    image: linuxserver/sonarr:4.0.16

    container_name: sonarr

    networks:

      - mynetwork   # Comment this line if VPN is enabled

    ## Uncomment below lines if VPN is enabled

    #   mynetwork:

    #     ipv4_address: ${SONARR_STATIC_CONTAINER_IP} # It should be available IPv4 address in range of docker network `mynetwork` e.g. 172.20.0.2

    environment:

      - PUID=1000

      - PGID=1000

      - TZ=Europe/Zurich

    volumes:

      - sonarr-config:/config

      - /mnt/watch:/downloads

    ports:

      - 8989:8989

    restart: unless-stopped

  

  prowlarr:

    profiles: ["vpn", "no-vpn"]

    container_name: prowlarr

    image: linuxserver/prowlarr:2.3.0

  

    # Uncomment below if vpn is enabled

    # depends_on:               # Uncomment this line if vpn is enabled

      # vpn:                   # Uncomment this line if vpn is enabled

      #   condition: service_healthy # Uncomment this line if vpn is enabled

    # network_mode: service:vpn # Uncomment this line if vpn is enabled

  

    networks:               # Comment this line if vpn is enabled

      - mynetwork           # Comment this line if vpn is enabled

    environment:

      - PUID=1000

      - PGID=1000

      - TZ=Europe/Zurich

    volumes:

      - prowlarr-config:/config

  

    # Comment below ports if VPN is enabled.

    ports:

      - 9696:9696

    restart: unless-stopped

  

  #recommendarr:

  #  profiles: ["recommendarr"]

  #  container_name: recommendarr

  #  image: tannermiddleton/recommendarr:v1.4.4

  #  networks:

  #    - mynetwork

  #  environment:

  #    - NODE_ENV=production

  #    - DOCKER_ENV=true

  #    - PORT=3000

  #    - PUBLIC_URL=https://localhost:3000 # Change this public URL if you are accessing recommendarr on a domain

  #  volumes:

  #    - recommendarr-data:/app/server/data

  #  ports:

  #    - 3000:3000

  #  restart: unless-stopped

  

  jellyseerr:

    profiles: ["vpn", "no-vpn"]

    image: ghcr.io/seerr-team/seerr:v3.0.1

    container_name: seerr

    networks:

      - mynetwork

    environment:

      - PUID=1000

      - PGID=1000

      - TZ=Europe/Zurich

      - PORT=5055

    volumes:

      - seerr-config:/app/config

    ports:

      - 5055:5055

    restart: unless-stopped

  

  jellyfin:

    profiles: ["vpn", "no-vpn"]

    image: linuxserver/jellyfin:10.11.6

    container_name: jellyfin

    networks:

      - mynetwork

    environment:

      - PUID=1000

      - PGID=1000

      - TZ=Europe/Zurich

    volumes:

      - jellyfin-config:/config

      - /mnt/watch:/data

  # devices:

   #  - /dev/videoN:/dev/videoN # Mount GPU device

    ports:

      - 8096:8096

      - 7359:7359/udp

      - 8920:8920

    restart: unless-stopped

  # Doc: https://github.com/navilg/cleanmyarr

  # cleanmyarr:

  #   profiles: ["vpn", "no-vpn"]

  #   depends_on:

  #     - radarr

  #     - sonarr

  #   image: linuxshots/cleanmyarr:0.8.1

  #   container_name: cleanmyarr

  #   networks:

  #     - mynetwork

  #   volumes:

  #     - cleanmyarr-config:/config

    # restart: unless-stopped

    # environment:

    # - CMA_MAINTENANCE_CYCLE=${CMA_MAINTENANCE_CYCLE:-""}

    # - CMA_DELETE_AFTER_DAYS=${CMA_DELETE_AFTER_DAYS:-""}

    # - CMA_ENABLE_EMAIL_NOTIFICATION=${CMA_ENABLE_EMAIL_NOTIFICATION:-""}

    # - CMA_SMTP_USERNAME=${CMA_SMTP_USERNAME:-""}

    # - CMA_SMTP_ENCODED_PASSWORD=${CMA_SMTP_ENCODED_PASSWORD:-""}

    # - CMA_SMTP_TO_EMAILS=${CMA_SMTP_TO_EMAILS:-""}

    # - CMA_ENABLE_GOTIFY_NOTIFICATION=${CMA_ENABLE_GOTIFY_NOTIFICATION:-""}

    # - CMA_GOTIFY_URL=${CMA_GOTIFY_URL:-""}

    # - CMA_GOTIFY_ENCODED_APP_TOKEN=${CMA_GOTIFY_ENCODED_APP_TOKEN:-""}

    # - CMA_ENABLE_TELEGRAM_NOTIFICATION=${CMA_ENABLE_TELEGRAM_NOTIFICATION:-""}

    # - CMA_TELEGRAM_ENCODED_BOT_TOKEN=${CMA_TELEGRAM_ENCODED_BOT_TOKEN:-""}

    # - CMA_TELEGRAM_CHAT_ID=${CMA_TELEGRAM_CHAT_ID:-""}

    # - CMA_MONITOR_RADARR=${CMA_MONITOR_RADARR:-""}

    # - CMA_RADARR_URL=${CMA_RADARR_URL:-""}

    # - CMA_RADARR_ENCODED_API_KEY=${CMA_RADARR_ENCODED_API_KEY:-""}

    # - CMA_RADARR_ENABLE_NOTIFICATION=${CMA_RADARR_ENABLE_NOTIFICATION:-""}

  

volumes:

  gluetun-data:

  radarr-config:

  sonarr-config:

  prowlarr-config:

  jellyfin-config:

  qbittorrent-config:

  seerr-config:

  recommendarr-data:

  transmission-config:

  # cleanmyarr-config:

  

networks:

  mynetwork:

    external: true
```

Die Datei muss nun in einem Order der Wahl abgespeichert werden, und mit dem folgenden Command ausgeführt werden. (Dies, damit die Variante ohne VPN ausgeführt wird)

```shell
docker compose --profile no-vpn up -d
```

Das herunterziehen und starten der Images/Container kann einen Moment dauern. 
![](Pasted%20image%2020260224210725.png)


# Konfiguration der Verschiedenen Services
## Konfiguration Transmission
Hier muss nichts Konfiguriert werden, die gesamte Konfiguration wurde bereits im Compose erledigt. 
## Konfiguration Radarr
- Webseite öffnen http://192.168.x.x:7878/
- Admin Account erstellen![](Pasted%20image%2020260224214011.png)


Anschliessend unter "Settings", "Media Management" den Haken setzen und Speichern. 
![](Pasted%20image%2020260224214412.png)


Root Folder auf "/downloads/complete" setzen
![](Pasted%20image%2020260224230634.png)


Unter "Settings", "Download Clients" einen neuen Download Client hinzufügen.
![](Pasted%20image%2020260224230817.png)


"Transmission" auswählen
![](Pasted%20image%2020260224230900.png)

Anschliessend die korrekten daten von Transmission angeben
**Achtung:** Man muss die IP-Adresse des Server verwenden, localhost funktioniert nicht (Test schlägt fehl) 
![](Pasted%20image%2020260224231457.png)
![](Pasted%20image%2020260224231550.png)

Transmission sollte nun unter den Download Clients angezeigt werden. 



**Probleme**
Drecks Browsercache..........

## Konfiguration Sorarr
Die Konfiguration in Sonarr ist gleich wie in Radarr, desswegen verzichte ich auf eine weitere Erläuterung. 
![](Pasted%20image%2020260224224615.png)
![](Pasted%20image%2020260224224734.png)

Unter Setting, Profiles die nicht benötigten Services löschen. ![](Pasted%20image%2020260224232401.png)![](Pasted%20image%2020260224232433.png)

Download Client hinzufügen, wieder Transmission, genauso wie bei Radarr
![](Pasted%20image%2020260224232531.png)
![](Pasted%20image%2020260225233824.png)



## Konfiguration Jellyfin

Servername eingeben
![](Pasted%20image%2020260224215408.png)

Admin Benutzer erstellen
![](Pasted%20image%2020260224215336.png)

Neue Medienbibliothek hinzufügen
![](Pasted%20image%2020260224232844.png)

Inhaltstyp Festlegen, und den korrekten Pfand wählen, wo die Filme gespeichert sind.  
![](Pasted%20image%2020260224233112.png)

Das gleiche nun noch für Serien (Ordner tvshows)

![](Pasted%20image%2020260224233235.png)

Die weiteren Festen können auf der Standarteinstellung belassen werden. 
![](Pasted%20image%2020260224233258.png)![](Pasted%20image%2020260224233319.png)
![](Pasted%20image%2020260224233339.png)
## Konfiguration Seerr

seerr ist der nachfolger von Jellyseer. 

Als Servertyp kann "Jellyfin" ausgewählt werden. 
![](Pasted%20image%2020260224234057.png)

Dann können die eingerichteten Libarys aus Jellyfin synchronisiert werden. Auch die externe URL muss angegeben werden. 
![](Pasted%20image%2020260224234520.png)

Danach müssen die Radarr und Sonarr Server verbunden werden
![](Pasted%20image%2020260224234611.png)


Der API Key ist in Radarr/Sonarr unter "Settings", "General" zu finden. 
![](Pasted%20image%2020260224234857.png)

Das gleiche muss nun noch für Sonarr gemacht werden. 


Danach landet man auf der Übersichtsseite von Seerr. Nun ist diese Konfiguration abgeschlossen. 
![](Pasted%20image%2020260224235128.png)


## Konfiguration Prowlarr

Admin Account erstellen
![](Pasted%20image%2020260224235611.png)

Unter "Indexers", "Add Indexer" ein Indexer hinzufügen. 
![](Pasted%20image%2020260224235746.png)

Am besten verwendet man PirateBay. 
![](Pasted%20image%2020260224235935.png)

Damit Radarr und Sonarr nun wissen, von wo sie ihre Filme/Serien herunterladen sollen, müssen diese nun noch unter "Settings", "Apps" hinzugefügt werden. 
![](Pasted%20image%2020260225000156.png)

![](Pasted%20image%2020260225000228.png)

Auch hier sollte wieder die IP und nicht "localhost" verwendet werden. 
![](Pasted%20image%2020260225000357.png)

Wenn es fertig eingerichtet ist, sieht es so aus:
![](Pasted%20image%2020260225000500.png)

Der MediaStack sollte nun Theoretisch funktionieren. Ich habe aber aktuell noch das Problem, dass Radarr die Filme, welche Transmission heruntergeladen hat nicht sieht. Ich denke es liegt an den Mountpunkten, aber die Zeit dies zu beheben hat nicht mehr gereicht vor der Abgabe der Dokumentation. 








