# 03 — User and Access Management

## Why this step

Identity and privilege controls are foundational. Most compromises become severe when privilege boundaries are weak.

## 1) Create an administrative user (if needed)

```bash
sudo adduser secadmin
sudo usermod -aG sudo secadmin
id secadmin
```

## 2) Enforce strong authentication hygiene

- Use unique passphrases/passwords
- Prefer SSH keys over passwords
- Avoid shared accounts

## 3) Review sudo privileges

```bash
sudo -l -U secadmin
sudo grep -R "NOPASSWD" /etc/sudoers /etc/sudoers.d 2>/dev/null
```

### Why
Overly broad sudo rights can turn minor account compromise into full system compromise.

## 4) Disable direct root SSH login (implemented in SSH hardening)

Root should not authenticate directly over SSH.  
Use named user + sudo for accountability and reduced attack clarity.