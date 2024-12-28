# Software

## Base setup

-   ### Raspberry Pi
    -   Raspberry Pi OS Lite 64bit
-   ### Medion
    -   Ubuntu 22.04.4 LTS x86_64 (Server)
    -   Docker

## Hosted Services

### Dockerfile
-   Configuration: see docker-compose.yml
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
    -   `https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/samsung`
    -   `https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/Fake-Science`
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
