# 04 — SSH Hardening

## Why this step

SSH is usually the primary remote entry point. Most internet-exposed servers receive constant SSH probing.

## 1) Generate SSH key pair on client machine

```bash
ssh-keygen -t ed25519 -C "secadmin@your-server"
```

## 2) Copy public key to server

```bash
ssh-copy-id secadmin@<SERVER_IP>
```

## 3) Edit SSH daemon config

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config
```

Recommended settings:

```conf
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding no
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
AllowUsers secadmin
```

> Keep `UsePAM yes` unless you have a specific reason and tested alternative.

## 4) Validate and reload safely

```bash
sudo sshd -t
sudo systemctl reload ssh
sudo systemctl status ssh --no-pager
```

## 5) Critical lockout prevention

- Open a **new terminal** and test SSH key login.
- Only after success, close old session.
- If locked out, use console access and restore backup:

```bash
sudo cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
sudo systemctl restart ssh
```

## Why these settings

- `PermitRootLogin no`: removes direct root brute-force path
- `PasswordAuthentication no`: defeats password guessing attacks
- `AllowUsers`: limits who can attempt login
- `MaxAuthTries`: reduces brute-force per connection effectiveness