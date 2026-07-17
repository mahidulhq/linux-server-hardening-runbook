# 05 — Firewall and Network Hardening

## Why this step

Even secure services should not be exposed unless necessary. Firewall rules enforce network-level least privilege.

## 1) Default deny inbound, allow outbound

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

## 2) Allow required services only

```bash
sudo ufw allow OpenSSH
# Example for web server:
# sudo ufw allow 80/tcp
# sudo ufw allow 443/tcp
```

If possible, restrict SSH source IP:

```bash
sudo ufw delete allow OpenSSH 
sudo ufw allow 22/tcp #OR TRY THIS for openssh
sudo ufw allow from <TRUSTED_IP>/32 to any port 22 proto tcp
```

## 3) Enable firewall

```bash
sudo ufw enable
sudo ufw status verbose
```

## 4) Verify listening services

```bash
ss -tulpen
```

### Why
If a service is listening unexpectedly, it increases attack surface even if not immediately exploitable.