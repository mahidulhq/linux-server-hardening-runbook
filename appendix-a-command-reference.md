# Appendix A — Command Reference

## System baseline
```bash
hostnamectl
uname -a
lsb_release -a
ip a
ss -tulpen
```

## Package management
```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

## SSH
```bash
dpkg -l | grep openssh-server
sudo systemctl enable --now ssh
sudo sshd -t
sudo systemctl reload ssh
sudo systemctl status ssh --no-pager
```

## UFW
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status verbose
```

## Fail2ban
```bash
sudo apt install -y fail2ban
sudo systemctl enable --now fail2ban
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

## Sysctl
```bash
sudo sysctl --system
sudo sysctl net.ipv4.tcp_syncookies
```

## Logs and audit
```bash
sudo journalctl -u ssh -n 100 --no-pager
sudo journalctl -p warning --since "24 hours ago" --no-pager
sudo apt install -y auditd audispd-plugins
sudo auditctl -s
sudo ausearch -m USER_LOGIN -ts today
```

## Backup
```bash
sudo tar -czf /root/hardening-config-backup-$(date +%F).tar.gz \
  /etc/ssh/sshd_config /etc/fail2ban/jail.local /etc/ufw /etc/sysctl.d/99-hardening.conf
```