# PKI

## Certificate Authority Structure

```text
Kaiden Homelab Root CA
        │
        ▼
Kaiden Homelab Issuing CA
        │
        ├── vaultwarden.lab.lan
        ├── adguard.lab.lan
        ├── nas.lab.lan
        └── proxmox.management.lan
```

## Trusted Devices

- Blackbox Desktop
- Windows Laptop
- iPhone

## Certificate Issuance Workflow

1. Generate Key
2. Generate CSR
3. Sign with Intermediate CA
4. Build Full Chain
5. Deploy Certificate

# Certificate Issuance

## PKI Hierarchy

```text
Kaiden Homelab Root CA
        │
        ▼
Kaiden Homelab Issuing CA
        │
        ├── ${VAULTWARDEN_FQDN}
        ├── ${ADGUARD_FQDN}
        ├── ${NAS_FQDN}
        ├── ${PROXMOX_FQDN}
        └── future services
```

---

# New Service Certificate

>T

## 1. Create Certificate Folder

```bash
cd /mnt/homelab-pki/issued-certs

mkdir immich.lab.lan

cd immich.lab.lan
```

---

## 2. Generate Private Key

```bash
openssl genrsa -out immich.lab.lan.key.pem 2048
```

---

## 3. Create CSR Configuration

Create:

```bash
nano immich.lab.lan.cnf
```

Contents:

```ini
[req]
default_bits = 2048
prompt = no
default_md = sha256
req_extensions = req_ext
distinguished_name = dn

[dn]
C = US
ST = Wisconsin
L = Milwaukee
O = Kaiden Homelab
OU = Services
CN = immich.lab.lan

[req_ext]
subjectAltName = @alt_names

[alt_names]
DNS.1 = immich.lab.lan
```

---

## 4. Generate CSR

```bash
openssl req \
-new \
-key immich.lab.lan.key.pem \
-out immich.lab.lan.csr.pem \
-config immich.lab.lan.cnf
```

---

## 5. Create Extension File

Create:

```bash
nano immich.lab.lan.ext
```

Contents:

```ini
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage=digitalSignature,keyEncipherment
extendedKeyUsage=serverAuth
subjectAltName=DNS:immich.lab.lan
```

---

## 6. Sign Certificate

```bash
openssl x509 -req \
-in immich.lab.lan.csr.pem \
-CA ../../intermediate-ca/intermediate-ca.crt.pem \
-CAkey ../../intermediate-ca/intermediate-ca.key.pem \
-CAcreateserial \
-out immich.lab.lan.crt.pem \
-days 825 \
-sha256 \
-extfile immich.lab.lan.ext
```

---

## 7. Build Full Chain

```bash
cat immich.lab.lan.crt.pem \
../../intermediate-ca/intermediate-ca.crt.pem \
> fullchain.pem
```

---

## 8. Verify Certificate

```bash
openssl verify \
-CAfile ../../root-ca/root-ca.crt.pem \
-untrusted ../../intermediate-ca/intermediate-ca.crt.pem \
immich.lab.lan.crt.pem
```

Expected:

```text
immich.lab.lan.crt.pem: OK
```

---

## Generated Files

```text
immich.lab.lan/
├── immich.lab.lan.key.pem
├── immich.lab.lan.crt.pem
├── immich.lab.lan.csr.pem
├── immich.lab.lan.cnf
├── immich.lab.lan.ext
└── fullchain.pem
```

---

# Deploying A Certificate

Most services require:

```text
fullchain.pem
```

and

```text
service.key.pem
```

Examples:

| Service | Certificate |
|----------|----------|
| Vaultwarden | fullchain.pem |
| AdGuard | fullchain.pem |
| OMV | fullchain.pem |
| Proxmox | fullchain.pem |

---

# New Device Trust

## Windows

### Import Root CA

Open:

```text
certlm.msc
```

Navigate:

```text
Trusted Root Certification Authorities
```

Import:

```text
root-ca.crt.pem
```

Verify:

```powershell
Get-ChildItem Cert:\LocalMachine\Root |
Where-Object {$_.Subject -like "*Kaiden*"}
```

---

## iPhone

### Install Root CA

1. Copy:

```text
root-ca.crt.pem
```

to iPhone.

2. Open certificate.

3. Install profile.

4. Navigate:

```text
Settings
→ General
→ About
→ Certificate Trust Settings
```

5. Enable:

```text
Kaiden Homelab Root CA
```

---

## Android

### Install Root CA

Navigate:

```text
Settings
→ Security
→ Install Certificate
```

Select:

```text
root-ca.crt.pem
```

Trust certificate.

---

## New Laptop Checklist

```text
□ Copy root-ca.crt.pem
□ Import Root CA
□ Verify Vaultwarden HTTPS
□ Verify AdGuard HTTPS
□ Verify OMV HTTPS
□ Verify Proxmox HTTPS
```

Test:

```text
https://vaultwarden.lab.lan
https://adguard.lab.lan
https://nas.lab.lan
https://proxmox.management.lan:8006
```

Expected:

```text
Connection Secure
Verified by Kaiden Homelab Root CA
```

---

# Certificate Renewal

Current Certificate Lifetime:

```text
825 Days
```

Renewal Process:

```text
1. Generate New Certificate
2. Deploy New Certificate
3. Restart Service
4. Verify HTTPS
```

Root CA and Intermediate CA remain unchanged.