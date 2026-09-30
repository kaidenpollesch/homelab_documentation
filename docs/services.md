# Services

## Proxmox (Type 1, Baremetal HyperVisor)

**Macbook 2017 Total Resources**:

- CPU Cores: 4
- RAM: 16 GB
- Storage: 500 GB

**Purpose**:

- Virtualization Platform

**HTTPS**:

- Internal CA Certificate

---

## AdGuard Home (LXC)

>Deployment for this service was an incremental rollout over all VLANs, where I started with the Lab VLAN and just my PC, then added more VLANs. I didn't want to put my home network down without testing it for a couple of days first.

**OS**: `debian-13-standard_13.6-1_amd64.tar.zst`

**VMID**: `${ADGUARD_VMID}`

**Resoureces**:

- CPU Cores Allocated: 1
- RAM Allocated: 512 MB
- Storage Allocated: 8 GB

**VLAN**: ${LAB_VLAN}

**Purpose**:

- DNS Filtering
- Local DNS Records
- DNS Rewrites to point to important IP addresses
- Network Wide Ad Blocker

**HTTPS**:

- Internal CA Certificate

---

## Vaultwarden (LXC)

**OS**: `debian-13-standard_13.6-1_amd64.tar.zst`

**VMID**: `${VAULTWARDEN_VMID}`

**Resoureces**:

- CPU Cores Allocated: 1
- RAM Allocated: 2 GB
- Storage Allocated: 16 GB

**VLAN**: ${LAB_VLAN}

**Purpose**:

- Local Password Management

**Features**:

- HTTPS
- Nginx Reverse Proxy
- Automated Backups
- 2FA Enabled

---

## OpenMediaVault (Virtual Machine)

**OS**: `iso/openmediavault_8.3.1-amd.iso`

**VMID**: `${NAS_VMID}`

**Resoureces**:

- CPU Cores Allocated:  2
- RAM Allocated: 2 GB
- Storage Allocated: 200 GB

**VLAN**: ${LAB_VLAN}

**Purpose**:

- NAS
- SMB Shares
- Backup Storage

**HTTPS**:

- Internal CA Certificate

---

## Minecraft (Virtual Machine)

**OS**: `iso/debian-13.6-0_amd64-netinst.iso`

**VMID**: `${MINECRAFT_VMID}`

**Resoureces**:

- CPU Cores Allocated: 4
  - Ballooning Device=off
- RAM Allocated: 6 GB
- Storage Allocated: 64 GB

**VLAN**: ${DMZ_VLAN}

**Purpose**:

- Game Server

**UFW Firewall Rules**:

- Deny Incoming
- Allow Outgoing
- Allow SSH
- Allow Minecraft Port
