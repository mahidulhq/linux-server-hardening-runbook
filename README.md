# Linux Server Hardening (Ubuntu)

A practical, beginner-friendly project to harden an Ubuntu server using security best practices.

---

## Table of Contents

- [Project Overview](00-project-overview.md)
- [Threat Model & Security Goals](/01-threat-model-and-security-goals.md)
- [Baseline & Preparation](/02-baseline-and-preparation.md)
- [User & Access Management](03-user-and-access-management.md)
- [SSH Hardening](04-ssh-hardening.md)
- [Firewall & Network Hardening](05-firewall-network-hardening.md)
- [System Updates & Patch Management]( 06-system-updates-and-patch-management.md)
- [Intrusion Prevention (Fail2ban)](07-intrusion-prevention-fail2ban.md)
- [Kernel & Runtime Hardening]( 08-kernel-and-runtime-hardening.md)
- [Logging, Monitoring & Auditing]( 09-logging-monitoring-auditing.md)
- [Backup & Recovery]( 10-backup-and-recovery.md)
- [Set password and auth policies (PAM)]( 11-password-auth-policies.md)
- [Operations & Maintenance]( 12-operations-and-maintenance.md)
- [Troubleshooting]( 13-troubleshooting.md)
- [Future Improvements]( 14-future-improvements.md)
- [Appendix A: Command Reference]( appendix-a-command-reference.md)
- [Appendix B: Configuration Backup]( appendix-b-configuration-backup.md)
- [Validation and Security Checklist](validation-and-security-checklist.md)
---

## Scope

This project focuses on:
- Initial server baseline and inventory
- SSH hardening with key-based authentication
- Firewall and network controls
- Brute-force mitigation (Fail2ban)
- Automatic security updates
- Logging and auditing basics
- Backup and recovery readiness
- Validation checklist and ongoing maintenance

---

## Quick Start

1. Read [Project Overview]( 00-project-overview.md)
2. Follow the guides in order (`00` → `13`)
3. Execute commands carefully and test access after each critical change
4. Complete [Validation & Security Checklist]( 11-validation-and-security-checklist.md)

---

## Safety Notes

- **Always keep one active root/console session** while hardening SSH and firewall settings.
- Test new SSH config in a second session before disconnecting your original session.
- Back up config files before changes (see Appendix B).
- Do not disable password auth until SSH keys are verified.