# 09 — Logging, Monitoring, and Auditing

## Why this step

Without logs, incidents are hard to detect, investigate, or contain.

## 1) Use journalctl for system/SSH visibility

```bash
sudo journalctl -u ssh -n 100 --no-pager
sudo journalctl -p warning --since "24 hours ago" --no-pager
```

### View System Errors `journalctl`
This command shows only high-severity system errors (errors, critical, alerts, and emergencies) since the last boot:

```bash
sudo journalctl -p 3 -xb
```
### Monitor SSH & Auth Logs in Real-Time `tail`
This command lets you watch authentication attempts live as they happen.

```bash
sudo tail -f /var/log/auth.log
```
Note: On very recent Ubuntu installations, auth logs may be routed entirely through systemd. If the file /var/log/auth.log doesn't exist, use this command to stream live auth logs instead:

```bash
sudo journalctl -f -u ssh
```
### Search Audit Logs for Logins (ausearch)
To query the audit daemon specifically for user authentication events (both successful and failed):

```bash
sudo ausearch -m USER_AUTH
```

## Install logwatch (Optional)
logwatch is a customizable log analysis system. It goes through your system logs over a given period and creates a clean, human-readable report analyzing what happened on your server.

Install the utility:
```bash
sudo apt install logwatch -y
```
Generate your first daily report:
```bash
sudo logwatch --detail High --range Today --output stdout
```

## 2) Install and configure auditd

```bash
sudo apt install -y auditd audispd-plugins
sudo systemctl enable --now auditd
sudo systemctl status auditd --no-pager
```

Basic checks:

```bash
sudo auditctl -s
sudo ausearch -m USER_LOGIN -ts today
```

## 3) Monitor auth logs (if present)

```bash
sudo tail -f /var/log/auth.log
```

## 4) File permissions and account checks

Audit sensitive files:
```bash
ls -l /etc/passwd /etc/shadow /etc/group /etc/gshadowsudo stat /etc/shadow
```
Expected important ownership:

```bash
/etc/shadow should be root-owned and not world-readable
```
Find world-writable files (excluding proc/sys/run noise):

```bash
sudo find / -xdev -type f -perm -0002 -print
```
Find users with UID 0:

```bash
awk -F: '($3 == 0) {print}' /etc/passwd
```
Lock unused accounts (example):

```bash
sudo passwd -l usernamesudo usermod -s /usr/sbin/nologin username
```

## 5) Malware/rootkit basic checks

### Install the Scanning Utilities:

```bash
sudo apt install rkhunter chkrootkit -y
```

### Update and Run Rootkit Hunter `rkhunter`
`rkhunter` works by comparing your system files against a database of known rootkit signatures, as well as verifying that core system binaries haven't been altered.

Update the local signature database:
```bash
sudo rkhunter --update
```
Before running the check, you want rkhunter to take a snapshot of your current, clean system files so it knows what "normal" looks like:

```bash
sudo rkhunter --propupd
```
Run the system check:

```bash
sudo rkhunter --check --sk
```
### Run `chkrootkit`
chkrootkit performs a different style of analysis by looking through the system's process tables, memory spaces, and log entries for signs of active modification or hidden folders.

Run the binary scan:
```bash
sudo chkrootkit
```

##  6) Time sync

Check Current Status:
```bash
timedatectl status
```
Enable Network Time Protocol (NTP):
```bash
sudo timedatectl set-ntp true
```
Set Your Correct Timezone:
```bash
timedatectl list-timezones
sudo timedatectl set-timezone America/New_York
```

## 7) Verify hardening with scanners `Lynis`

Install Lynis:

```bash
sudo apt install lynis -y
```

Run the Comprehensive System Audit:
```bash
sudo lynis audit system
```

Analyze Your Results: The scan will pause occasionally or output results categorized by color-

`OK` or `Found` (Green) – Good configuration.  
`Suggestion` (Yellow) – Optional configuration to improve security.   
`Warning` (Red) – A potential security gap that needs immediate attention.  

Lynis stores its report details locally, but it helpfully dumps all warnings and suggestions at the end of the terminal output. To view them anytime without re-running the full scan, use `grep`:

```bash
sudo grep -E "warning|suggestion" /var/log/lynis-report.dat
```

## Why
- SSH and auth logs reveal intrusion attempts
- Audit trail improves accountability
- Early detection reduces incident impact