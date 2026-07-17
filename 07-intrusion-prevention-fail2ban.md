# 07 — Intrusion Prevention (Fail2ban)

## Why this step

Fail2ban blocks repeated malicious authentication attempts and reduces brute-force noise/risk.

## 1) Install Fail2ban

```bash
sudo apt install -y fail2ban
```

## 2) Configure local jail

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

Minimal SSH jail example:

```ini
[sshd]
enabled = true
port = 22
logpath = %(sshd_log)s
backend = systemd
maxretry = 5
findtime = 10m
bantime = 1h
```
or try
```ini
[DEFAULT]
# Ban the abusive IP for 1 hour
bantime = 1h

# Look for repeat failures within a 10-minute window
findtime = 10m

# Ban the IP after 5 failed attempts
maxretry = 5

# Use systemd journal to monitor logs (perfect for modern Ubuntu)
backend = systemd

[sshd]
enabled = true
port = ssh
```

## 3) Start and verify

```bash
sudo systemctl enable --now fail2ban
sudo systemctl status fail2ban --no-pager
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

## Why these values

- `maxretry=5`: allows minor human mistakes
- `findtime=10m`: detects concentrated attempts
- `bantime=1h`: meaningful temporary deterrence