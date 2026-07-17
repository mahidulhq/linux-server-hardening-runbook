# 00 — Project Overview


## Purpose

This project provides a comprehensive, structured, and practical runbook for hardening an Ubuntu Server instance. While designed to be accessible to junior administrators and security enthusiasts, the controls applied here reflect industry-standard defense-in-depth principles.

### Why Hardening Matters

A default server installation is functional, but not production-secure by itself. Hardening systematically modifies system parameters to reduce:

* **Unauthorized Access Risk:** Eliminates credential-based entry vectors by moving away from legacy password protocols.
* **Brute-Force and Automated Scanning Success:** Automatically drops hostile traffic at the network edge before it can saturate services.
* **Attack Surface from Unnecessary Services:** Prunes legacy applications and background listeners that present potential entry points.
* **Impact of Misconfiguration:** Mitigates the radius of an application compromise by implementing structural kernel and filesystem restrictions.



## Objectives

* **Create a Secure Baseline Configuration:** Establish a known, updated, and clean operational system state.
* **Apply the Principle of Least Privilege:** Strictly restrict root usage and enforce clear privilege boundaries for administrative accounts.
* **Secure Remote Access (SSH):** Implement rigid, key-only cryptographic authentication while disabling password and root login pathways.
* **Restrict Inbound Traffic:** Enforce a strict stateful firewall profile that drops unmapped incoming connections by default.
* **Enable Visibility (Logs, Auditing):** Deploy host-level audit engines to capture system calls and facilitate forensic review.
* **Ensure Recoverability (Backups):** Create secure, off-server configuration archives to guarantee a reliable rollback vector.



## High-Level Sequence

The hardening blueprint is executed in a strategic sequence, ensuring that connection pathways are secured and validated before structural system and kernel updates are loaded:

```
Preparation   
Access Controls 
Network Defense 
Kernel Hardening
Auditing & Backups
```

1. **Baseline and Package Updates:** Inventory system state, update core dependencies, and ensure baseline software health.
2. **User and Privilege Setup:** Isolate administrative responsibilities to a non-root sudo user and audit global execution parameters.
3. **SSH Hardening:** Implement an unbreachable asymmetric key profile, restrict user traffic, and mitigate session exposure.
4. **Firewall Hardening:** Build a stateful, default-deny perimeter rule set to drop arbitrary ingress scanning attempts.
5. **Brute-Force Mitigation:** Deploy an automated intrusion prevention system to parse logs and execute adaptive network blocks.
6. **Kernel/Runtime Security Controls:** Inject low-level parameters to eliminate network routing vulnerabilities like IP spoofing and redirect attacks.
7. **Logging and Monitoring:** Activate comprehensive host auditing daemons and complete baseline malware/rootkit scans.
8. **Backup and Recovery Drills:** Archive configurations to a clean, dated compressed file and transfer it off-host securely.
9. **Validation and Ongoing Operations:** Execute a unified post-deployment health check to verify system integrity and establish permanent maintenance cadences.