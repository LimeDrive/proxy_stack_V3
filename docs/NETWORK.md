# Networks

## Create all external networks

```bash
docker network create --driver=bridge --subnet=172.20.0.0/24 01_cloudflare_proxy
docker network create --driver=bridge --subnet=10.20.0.0/24 02_tailscale_local
```
***Note:*** *The IP ranges are not mandatory, but it is recommended to use them to avoid conflicts with other networks.*

---

## 01_cloudflare_proxy

This network is used to proxy all traffic through Cloudflare. It is used for all public facing services.
CNAMES are used to point to the correct service and need to be configured in Cloudflare.

### Range

`172.20.0.0/24`

### Services

#### [Traefik](https://traefik.io/) - Reverse proxy

- IP: `172.20.0.3`
- Port: `80`
- Port: `443`

#### [Authelia](https://www.authelia.com/) - Authentication

- IP: `172.20.0.2`

#### [Crowdsec](https://crowdsec.net/) - Intrusion detection

- IP: `172.20.0.4`

---

## 02_tailscale_local

This network is used to proxy all traffic through Tailscale, for all private services.
Tailscale is a VPN service that allows you to connect to your network from anywhere.
It need to be installed on all devices that need to access the private services. It is available for all major platforms.
Once the machine is connected to the VPN, you will get a private domain name for the machine.
Tailscale will automatically create a DNS entry for the machine and you will be able to access it using the domain name and path of the service.
HTTPS is automatically configured by Traefik using Tailscale's internal CA, to advoid conflicts with web browsers.

### Range

`10.20.0.0/24`

### Services

#### [Traefik](https://traefik.io/) - Reverse proxy

- IP: `10.20.0.3`
- Port: `80`
- Port: `443`

#### [Authelia](https://www.authelia.com/) - Authentication

- IP: `10.20.0.2`

#### [Crowdsec](https://crowdsec.net/) - Intrusion detection

- IP: `10.20.0.4`

### /!\ WorkInProgess /!\

This freture is work in progress so it's may have some bugs, and lot of things to improve.

---