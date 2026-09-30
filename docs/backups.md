# Backup Overview

>This document describes backup schedules, retention policies, storage locations, and restoration procedures.

## Backup Flow

```text
Vaultwarden
    │
    ▼
   NAS
    │
    ▼
Blackbox

Minecraft
    │
    ▼
   NAS
    │
    ▼
Blackbox

   PKI
    │
    ▼
   NAS
    │
    ▼
 Blackbox
```

# Backup Locations

## NAS

```text
Data/
└── backups/
    ├── vaultwarden/
    │   └── daily/
    └── minecraft/
```

## Blackbox

```text
C:\HomelabBackups\
├── vaultwarden\
├── minecraft\
└── homelab-pki\
```

# Vaultwarden

**Schedule**: `03:00 AM`

**Retention**: `30 Days`

**Backup Contents**: `/var/lib/vaultwarden/data`

Contains:

- SQLite Database
- Attachments
- Encryption Keys
- Vaultwarden Configuration

## Restore Vaultwarden

### Stop Vaultwarden

```bash
systemctl stop vaultwarden
```

### Backup Existing Data

```bash
mv /var/lib/vaultwarden/data \
/var/lib/vaultwarden/data.old
```

### Create New Directory

```bash
mkdir -p /var/lib/vaultwarden/data
```

### Extract Backup

Example:

```bash
tar -xzf vaultwarden-YYYY-MM-DD_HH-MM-SS.tar.gz -C /
```

### Verify

```bash
ls -lah /var/lib/vaultwarden/data
```

### Start Vaultwarden

```bash
systemctl start vaultwarden
```

### Verify

Open:

```text
https://${VAULTWARDEN_FQDN}
```

Verify:

- Login works
- Entries exist
- Attachments open

# Minecraft

**Schedule**: `04:00 AM`

## Restore Minecraft

### Stop Minecraft Server

```bash
systemctl stop minecraft
```

### Backup Existing World

```bash
mv world world.old
```

### Extract Backup

```bash
tar -xzf minecraft-YYYY-MM-DD.tar.gz
```

### Start Minecraft

```bash
systemctl start minecraft
```

### Verify

Connect to the server and verify:

- World loads
- Builds exist
- Players can join

---

# PKI

## Contents

```text
homelab-pki/
├── root-ca/
├── intermediate-ca/
├── issued-certs/
└── docs/
```

Contains:

- Root CA
- Intermediate CA
- Issued Certificates
- Certificate Documentation

---

## Restore PKI

### Copy Backup

Restore:

```text
homelab-pki/
```

to:

```text
\\nas.lab.lan\homelab-pki\
```

### Verify

Confirm the following files exist:

```text
root-ca/root-ca.key.pem
root-ca/root-ca.crt.pem

intermediate-ca/intermediate-ca.key.pem
intermediate-ca/intermediate-ca.crt.pem
```

### Test

```bash
openssl verify \
-CAfile root-ca/root-ca.crt.pem \
issued-certs/<certificate>/<certificate>.crt.pem
```

Expected: `OK`

# Disaster Recovery

## Proxmox Host Failure

Recovery Order:

```text
1. Reinstall Proxmox
2. Restore OMV
3. Restore AdGuard
4. Restore Vaultwarden
5. Restore Minecraft
```

---

## NAS Failure

Recovery Order:

```text
1. Restore NAS
2. Restore Data Share
3. Restore homelab-pki
4. Restore Vaultwarden Backups
5. Restore Minecraft Backups
```

---

## Vaultwarden Failure

Recovery Order:

```text
1. Rebuild LXC
2. Install Vaultwarden
3. Restore Backup Archive
4. Verify HTTPS
5. Verify Login
```

# Recovery Validation

After any restore:

## Verify HTTPS

```text
https://${VAULTWARDEN_FQDN}
https://${ADGUARD_FQDN}
https://${NAS_FQDN}
https://${PROXMOX_FQDN}:8006
```

Verify:

- Valid certificate
- Secure connection
- No browser warnings
