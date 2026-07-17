# 10 — Backup and Recovery

## Why this step

Security includes resilience. Misconfiguration, compromise, or hardware failure can happen.

## Backup scope (minimum)

- `/etc/ssh/sshd_config`
- `/etc/fail2ban/jail.local`
- `/etc/ufw/` rules
- `/etc/sysctl.d/99-hardening.conf`
- Application configs and critical data

## 1) Create local backup archive

```bash
sudo tar -czf /root/hardening-config-backup-$(date +%F).tar.gz \
  /etc/ssh/sshd_config \
  /etc/fail2ban/jail.local \
  /etc/ufw \
  /etc/sysctl.d/99-hardening.conf
```
To verify the file was created successfully and see its file size, check the /root directory:

```Bash
sudo ls -lh /root
```

## 2) Store backups off-server

- Remote storage, object storage, or another secure host
- Encrypt sensitive backups
- Restrict backup access permissions

## 3) Test restore procedure

A backup is only trustworthy if restoration is tested.

Example restore test (staging server):
1. Extract backup
2. Restore config files
3. Reload services
4. Validate SSH, firewall, Fail2ban

## Recovery objective

Define target:
- **RPO** (data loss tolerance)
- **RTO** (service restoration time)

Even basic documented targets improve incident response quality.