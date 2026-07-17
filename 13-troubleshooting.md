# 13 — Troubleshooting

## SSH access failed after config change

1. Use provider console access
2. Validate config:
   ```bash
   sudo sshd -t
   ```
3. Restore backup:
   ```bash
   sudo cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
   sudo systemctl restart ssh
   ```

## Locked out by UFW

From console:
```bash
sudo ufw status numbered
sudo ufw delete <RULE_NUMBER>
sudo ufw allow OpenSSH
sudo ufw reload
```

## Fail2ban not banning

- Confirm jail status:
  ```bash
  sudo fail2ban-client status sshd
  ```
- Check logs:
  ```bash
  sudo journalctl -u fail2ban -n 200 --no-pager
  ```
- Verify `backend` and `logpath` in `jail.local`

## Service failed after sysctl changes

- Re-open `/etc/sysctl.d/99-hardening.conf`
- Comment suspicious line
- Reapply:
  ```bash
  sudo sysctl --system
  ```

## High CPU or memory after hardening

- Identify process:
  ```bash
  top
  ps aux --sort=-%cpu | head
  ps aux --sort=-%mem | head
  ```
- Check if related service/log loop was introduced