# Software

## Base setup

-   ### Raspberry Pi
    -   Raspberry Pi OS Lite 64bit
-   ### Medion
    -   Ubuntu 22.04.4 LTS x86_64 (Server)
    -   Docker

## Hosted Services

### Dockerfile

```yml
services:
    nginxproxymanager:
        image: "jc21/nginx-proxy-manager:latest"
        container_name: nginxproxymanager
        restart: unless-stopped
        ports:
            - "80:80"
            - "81:81"
            - "443:443"
        volumes:
            - ./nginx/data:/data
            - ./nginx/letsencrypt:/etc/letsencrypt

    grafana:
        image: grafana/grafana:7.5.2
        restart: always
        ports:
            - 3000:3000
        volumes:
            - grafana:/var/lib/grafana
        depends_on:
            - influxdb

    influxdb:
        image: influxdb:1.8.3
        restart: always
        volumes:
            - influxdb:/var/lib/influxdb
        ports:
            - 8083:8083
            - 8086:8086
        environment:
            - INFLUXDB_ADMIN_USER="admin"
            - INFLUXDB_ADMIN_PASSWORD="password"
            - INFLUXDB_DB="speedtest"

    speedtest:
        image: robinmanuelthiel/speedtest:latest
        restart: always
        environment:
            - LOOP=true
            - LOOP_DELAY=1800
            - DB_SAVE=true
            - DB_HOST=http://influxdb:8086
            - DB_NAME=speedtest
            - DB_USERNAME=admin
            - DB_PASSWORD=password
        privileged: true # Needed for 'sleep' in the loop
        depends_on:
            - influxdb

    jellyfin:
        image: lscr.io/linuxserver/jellyfin:latest
        container_name: jellyfin
        environment:
            - PUID=1000
            - PGID=1000
            - TZ=Europe/Berlin
        volumes:
            - ./jellyfin/config:/config
            - ./jellyfin/tvshows:/data/tvshows
            - ./jellyfin/movies:/data/movies
            - ./jellyfin/shortfilms:/data/shortfilms
            - /mnt/fritznasmusic/Music/Musik:/data/music
            - /mnt/fritznasmusic/Sport:/data/sport
        devices:
            - /dev/dri/card0:/dev/dri/card0
            - /dev/dri/renderD128:/dev/dri/renderD128
        restart: unless-stopped

    glance:
        image: glanceapp/glance
        volumes:
            - ./glance/glance.yml:/app/glance.yml
            - /etc/timezone:/etc/timezone:ro
            - /etc/localtime:/etc/localtime:ro
        ports:
            - 8080:8080
        restart: unless-stopped

volumes:
    grafana:
    influxdb:
```

### Pi-Hole

-   Custom DNS-Server
-   Network-wide ad blocking & malicious site protection
-   total of 1,805,456 blocked domains (as of mid-October 2024)
-   used adlists:

    -   `https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts`
    -   `https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/fakenews-only/hosts`
    -   `https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/gambling-only/hosts`
    -   `https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/gambling-only/hosts`

    -   `https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/Win10Telemetry`

    -   `https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/DomainSquatting/DE/sonstige_Banken/Sparda-Bank`
    -   `https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/DomainSquatting/DE/sonstige_Banken/VolkswagenBank`
    -   `https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/DomainSquatting/DE/Volks-und-Raiffeisenbank/VR-PLZ-3-Teil-1`
    -   `https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/DomainSquatting/DE/Volks-und-Raiffeisenbank/VR-PLZ-3-Teil-2`

    -   `https://raw.githubusercontent.com/namePlayer/dhl-scamlist/main/dns-blocklists/pihole-blacklist`
    -   `https://raw.githubusercontent.com/danhorton7/pihole-block-tiktok/main/tiktok.txt`
    -   `https://raw.githubusercontent.com/laylavish/uBlockOrigin-HUGE-AI-Blocklist/refs/heads/main/noai_hosts.txt`

    -   `https://raw.githubusercontent.com/joseywoermann/pihole-blocklists/main/lists/chip/hosts`
    -   `https://raw.githubusercontent.com/joseywoermann/pihole-blocklists/main/lists/temu/hosts`
    -   `https://raw.githubusercontent.com/joseywoermann/pihole-blocklists/main/lists/misc/hosts`

-   whitelisted domains:

    -   `(\.|^)sparda-h\.de$`
    -   `wl.spotify.com`

-   Local DNS Records:
    | Domain | IP |
    |------------- |--------------- |
    | 1.fritz.box | 192.168.178.1 |
    | 2.fritz.box | 192.168.178.2 |
    | fritz.nas | 192.168.178.1 |

-   DNS Settings
    -   Upstream DNS Servers: IPv4/IPv6: Cloudflare
    -   Conditional Forwarding:
        -   Local network in CIDR: `192.168.178.0/24`
        -   IP of Router: `192.168.178.1`
        -   Local domain name: `fritz.box`

### Jellyfin

-   Local Media Server
-   Currently serving:
    -   34 movies
    -   940 episodes
    -   1915 songs

### Nginx Proxy Manager

-   SSL Certificate using Cloudflare & Let's Encrypt https://youtu.be/qlcVx-k-02E
-   Proxy Hosts:
    | Domain | Destination |
    |------------- |--------------- |
    | home.josey.dev | http://nginxproxymanager:81 |
    | dns.home.josey.dev | http://192.168.178.62:80 |
    | glance.home.josey.dev | http://glance:8080 |
    | grafana.home.josey.dev | http://grafana:3000 |
    | jellyfin.home.josey.dev | http://jellyfin:8096 |

### Grafana & InfluxDB

-   Currently only used to monitor my internet speed, no additional configuration aside from the docker compose needed

### Speedtest

-   see above

### Glance

-   New Tab Homepage

-   Configuration: see glance.yml
