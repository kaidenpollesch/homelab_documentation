# Homelab

Personal homelab built on Proxmox focused on learning systems administration, networking, virtualization, security, backups, and self-hosting.
> See `.env` file for spacific values formated like `${VALUE}`

## Goals

- Learn enterprise-style infrastructure
- Self-host critical services
- Practice Linux administration
- Implement secure networking
- Maintain reliable backups
- Document everything

## Core Technologies

- Proxmox VE
- Debian
- OpenMediaVault (OMV)
- AdGuard Home
- Vaultwarden
- WireGuard
- Nginx
- OpenSSL

## Infrastructure Overview

```text
Internet
    │
Router (Unifi Express 7)
    │
Managed Switch (USW Flex Mini)
    │
Proxmox Host (Macbook 2017)
    ├── OMV
    ├── AdGuard
    ├── Vaultwarden
    └── Minecraft
```

## Services

| Service | Purpose |
| ---------- | ---------- |
| Proxmox | Virtualization Platform |
| OMV | NAS and Storage |
| AdGuard | DNS Filtering |
| Vaultwarden | Password Management |
| WireGuard | Remote Access |
| Minecraft | Game Server |

## Documentation

- [Network](/docs/network.md)
- [Backups](/docs/backups.md)
- [PKI (Public Key Infrastructure)](/docs/pki.md)
- [Services](/docs/services.md)

## Future Plans

- Image Backup Hosting to get away from paid storage through iCloud and move to [Immich](https://immich.app/)
