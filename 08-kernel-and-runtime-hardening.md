# 08 — Kernel and Runtime Hardening

## Why this step

Kernel/network parameters can reduce exploitability of common network abuse patterns.

## 1) Configure sysctl hardening

Create dedicated config file:

```bash
sudo nano /etc/sysctl.d/99-hardening.conf
```

Add:

```conf
# IP spoofing protection
net.ipv4.conf.all.rp_filter=1
net.ipv4.conf.default.rp_filter=1

# Ignore ICMP redirects
net.ipv4.conf.all.accept_redirects=0
net.ipv4.conf.default.accept_redirects=0
net.ipv6.conf.all.accept_redirects=0
net.ipv6.conf.default.accept_redirects=0

# Disable source-routed packets
net.ipv4.conf.all.accept_source_route=0
net.ipv4.conf.default.accept_source_route=0
net.ipv6.conf.all.accept_source_route=0
net.ipv6.conf.default.accept_source_route=0

# Log suspicious packets
net.ipv4.conf.all.log_martians=1

# SYN flood protection
net.ipv4.tcp_syncookies=1
```

Apply:

```bash
sudo sysctl --system
```

## 2) Disable unused services

### 1. Audit What is Currently Running
Before we delete anything, let's see what your server is actually exposing to the network and what services start up automatically.

Check all open network ports:

```bash
ss -tulpen
```
### 2. List all services enabled at boot:

```bash
sudo systemctl list-unit-files --type=service | grep enabled
```

### 3. Purge Insecure and Legacy Software
Insecure protocols that transmit data (including passwords) in plaintext. They have no place on a hardened server.

Run this command to completely purge them (including their configuration files):

```bash
sudo apt purge -y telnet rsh-client rsh-redone-client talk ftp
```
run the system cleanup to wipe out any leftover software dependencies that are no longer needed:

```bash
sudo apt autoremove --purge -y
```

### 4. Disable what is unnecessary (example):

```bash
sudo systemctl disable --now <service_name>
```

> NOTE: Only disable services after confirming they are not required.

## Why
Reducing active components lowers attack surface and operational complexity.


## 3) Secure shared memory

Depending on how your system is set up, running sudo mount -o remount /run/shm might give you an error saying the mount point doesn't exist. This is because modern versions of Ubuntu use /dev/shm as the actual shared memory mount path, and make /run/shm a symbolic link to it.

If your command throws an error, adjust your path to the modern standard by running this instead:

```bash
echo 'tmpfs /dev/shm tmpfs defaults,noexec,nosuid,nodev 0 0' | sudo tee -a /etc/fstab
sudo mount -o remount /dev/shm
mount | grep shm
```

### Successful or not
When you run `mount | grep shm`, a successful result should print out a line containing your flags:

```bash
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,noexec,relatime)
```