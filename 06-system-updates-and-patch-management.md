# 06 — System Updates and Patch Management

## Why this step

Vulnerability management is a continuous process. Hardening is not one-time.

### Installation
```ini
sudo apt update
sudo apt install unattended-upgrades apt-listchanges -y
```

## 1) Enable automatic security updates

```bash
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

## 2) Verify configuration

### Check `20auto-upgrades` (The Schedule)

This file controls how often the system checks for updates. Open it to verify its contents:

```ini
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
Ensure it looks exactly like this (the 1 means "every 1 day"):
```
Ensure it looks exactly like this (the 1 means "every 1 day"):

```ini
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

### Check `50unattended-upgrades` (The Rules)

This is the master configuration file that defines which packages are allowed to auto-update (by default, it is configured to only install security updates, which is perfect because it prevents application-breaking changes).

Open it to look around:

```ini
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```
Process: Scroll down a bit. There is a section called `Unattended-Upgrade::Allowed-Origins`. Inside it, there is a line ending in "-security", that is uncommented (no // at the start). This means security patches are active.

Optional Hardening (Auto-Reboot): If a security patch updates the Linux kernel, the patch won't take effect until the system reboots. If you want your server to automatically reboot at 2:00 AM only when a reboot is required, search (Ctrl+W) for Automatic-Reboot and change the settings to:

```ini
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "02:00";
```

### Run a Dry-Run Test
To verify that your configuration is syntactically correct and that the upgrade engine can successfully communicate with Ubuntu's repositories, run a simulation (dry-run):

```ini
sudo unattended-upgrade --dry-run --debug
```
The command will take a moment to run. It will output a lot of diagnostic text.

Success Indicator: Look at the very last few lines of the output. If it completes without throwing Python errors or dependency failures, your automatic security update system is fully operational.


## 3) Manual patch routine (recommended)

- Weekly:
  ```bash
  sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
  ```
- Monthly:
  - Reboot planning for kernel/security updates
  - Validate core services after reboot

## Why both auto + manual

- Automatic updates reduce exposure window
- Manual cadence ensures review, cleanup, and operational verification