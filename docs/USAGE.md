# Proxy Stack V3 Usage.

## Dynamic configuration labels with traefik.

To use this stack in other docker-compose files you need to add the following label to your service app in others stack:

**Note:** *You can use only one of the following label block (Cloudflare or Tailscale), depending on your needs.*

```yaml
      # Diun
      - "diun.enable=true"
      # Traefik
      - "traefik.enable=true"
      # Cloudflare
      - "traefik.http.routers.service-cf.rule=Host(`service.${NDD_CLOUDFLARE:?err}`)"
      - "traefik.http.routers.service-cf.entrypoints=web,websecure"
      - "traefik.http.routers.service-cf.tls=true"
      - "traefik.http.routers.service-cf.tls.certresolver=cf"
      - "traefik.http.routers.service-cf.service=service-svc"
      - "traefik.http.routers.service-cf.middlewares=external_auth@file"
      # TailScale
      - "traefik.http.routers.service-ts.rule=Host(`${NDD_TAILSCALE:?err}`) && Path(`/service`)"
      - "traefik.http.routers.service-ts.entrypoints=web,websecure"
      - "traefik.http.routers.service-ts.tls=true"
      - "traefik.http.routers.service-ts.tls.certresolver=ts"
      - "traefik.http.routers.service-ts.service=service-svc"
      - "traefik.http.routers.service-ts.middlewares=local_auth@file"
      # Service
      - "traefik.http.services.service-svc.loadbalancer.server.port=9091"
```

The following network neef to be imported in your docker-compose file:

```yaml
networks:
  01_cloudflare_proxy:
    name: 01_cloudflare_proxy
    external: true
    ipam:
      driver: default
      config:
        - subnet: 172.20.0.0/24  # docker network create --driver=bridge --subnet=172.20.0.0/24 01_cloudflare_proxy
  02_tailscale_local:
    name: 02_tailscale_local
    external: true
    ipam:
      driver: default
      config:
        - subnet: 10.20.0.0/24  # docker network create --driver=bridge --subnet=10.20.0.0/24 02_tailscale_local
```

The network configuration is available in the [NETWORK.md](NETWORK.md) file.
Since it's external network, you need to create them before using them. So most of the information in the file is not needed, and just there for reference.