# 02 — Baseline and Preparation

## Why this step

Before hardening, we need:
- Current system state
- Access safety plan to avoid lockout
- Updated packages to reduce known vulnerabilities

## Environment Assumptions

- Ubuntu Server (LTS preferred)
- Sudo-enabled user or root console access
- Stable network connection

## 1) Record baseline information

```bash
hostnamectl
uname -a
lsb_release -a
ip a
ss -tulpen
sudo ufw status verbose
```

Document:
- OS version
- Listening ports
- Active services
- Current firewall state

## 2) Update system packages

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

### Why
Patching closes known vulnerabilities and should be one of the earliest controls.

## 3) Ensure OpenSSH server is installed

```bash
dpkg -l | grep openssh-server || sudo apt install -y openssh-server
sudo systemctl enable --now ssh
sudo systemctl status ssh --no-pager
```

### Why
SSH is the secure standard for remote administration. Hardening SSH is central to this project.

## 4) Create a rollback mindset

Before editing critical files:
- Back up original config (`cp file file.bak`)
- Keep one console/root session active
- Validate in a second SSH session before closing the first