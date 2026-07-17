# 11 — Set password and auth policies (PAM)

## Why this step
By configuring PAM (Pluggable Authentication Modules) and system defaults, you ensure that even if an administrator needs to change their password, they cannot choose a weak, easily crackable one. You are also enforcing a rotation policy so passwords aren't kept forever.

## Set password and auth policies (PAM)
Install quality module (if not present):

```bash
sudo apt install libpam-pwquality -y
```
Edit:

```bash
sudo nano /etc/security/pwquality.conf
```
Example baseline:

```ini
minlen = 14
minclass = 3
maxrepeat = 3
dictcheck = 1
```
## Set password aging defaults:

```bash
sudo nano /etc/login.defs
```

Recommended baseline:

```ini
PASS_MAX_DAYS 90
PASS_MIN_DAYS 1
PASS_WARN_AGE 7
```
Apply to existing user:

Changing `/etc/login.defs` only affects new accounts. To enforce these new rotation rules on your existing secadmin account, you use the chage utility.

Apply the policies:
```bash
sudo chage -M 90 -m 1 -W 7 secadmin
```
Verify that the changes took effect:

```bash
sudo chage -l secadmin
```