# Network Architecture

## Helpful IP Addresses

- ${PROXMOX_IP}
- ${ADGUARD_IP}
- ${NAS_IP}
- ${VAULTWARDEN_IP}
- ${MINECRAFT_IP}

## VLANs

| VLAN | Purpose | VLAN |
| -------- | ---------- | --- |
| Management | Infrastructure | ${MGNT_VLAN} |
| Home | Trusted Devices | ${HOME_VLAN} |
| IoT | Smart Devices | IoT_VLAN |
| Lab | Servers | ${LAB_VLAN} |
| DMZ | Public Services | ${DMZ_VLAN} |

>The naming conventions for our VM IDs are <br>
100-199 = Virtual Machines (VMs) <br>
200-299 = Containers (LXCs)

## DNS

AdGuard Home provides:

- Local DNS: ${DNS_SERVER}
- DNS Rewrites
- Ad Blocking

## Remote Access

WireGuard provides secure remote access into the homelab via Unifi built in VPN connection.

- Subnet = ${WIREGUARD_SUBNET}
- Phone IP = ${WIREGUARD_PHONE_IP}
- Macbook IP = ${WIREGUARD_MAC_IP}
- ThinkPad IP = ${WIREGUARD_THINKPAD_IP}
- Desktop IP = ${WIREGUARD_BLACKBOX_IP}
