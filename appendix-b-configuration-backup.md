# Appendix B — Configuration Backup and Restore

## Files to back up

- `/etc/ssh/sshd_config`
- `/etc/fail2ban/jail.local`
- `/etc/ufw/`
- `/etc/sysctl.d/99-hardening.conf`

## Backup commands

```bash
sudo mkdir -p /root/backups/hardening
sudo cp /etc/ssh/sshd_config /root/backups/hardening/sshd_config.$(date +%F)
sudo cp /etc/fail2ban/jail.local /root/backups/hardening/jail.local.$(date +%F)
sudo cp /etc/sysctl.d/99-hardening.conf /root/backups/hardening/99-hardening.conf.$(date +%F)
sudo tar -czf /root/backups/hardening/ufw.$(date +%F).tar.gz /etc/ufw
```

## Restore examples

```bash
sudo cp /root/backups/hardening/sshd_config.YYYY-MM-DD /etc/ssh/sshd_config
sudo systemctl restart ssh
```

```bash
sudo cp /root/backups/hardening/jail.local.YYYY-MM-DD /etc/fail2ban/jail.local
sudo systemctl restart fail2ban
```

```bash
sudo cp /root/backups/hardening/99-hardening.conf.YYYY-MM-DD /etc/sysctl.d/99-hardening.conf
sudo sysctl --system
```

## Validation after restore

```bash
sudo sshd -t
sudo systemctl status ssh --no-pager
sudo systemctl status fail2ban --no-pager
sudo ufw status verbose
```