# LimeBox - Proxy stack V3.

Docker-compose stack to create a reverse proxy with automatic SSL certificate generation and renewal using Cloudflare or/and Tailscale and secure with Authelia and CrowdSec.

## Introduction

This is a docker-compose stack that will create a reverse proxy with automatic SSL certificate generation and renewal using Let's Encrypt.
This stack use traefik as reverse proxy and authelia as authentication provider to secure your apps.
Once this stack are up and running you can use the network to connect your apps to the proxy, by simply adding the label below to your app in others stack.
