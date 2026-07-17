# 01 — Threat Model and Security Goals

## Threat Model 

Assume the server is reachable from the internet and faces:

- Automated SSH brute-force attempts
- Credential stuffing/password guessing
- Opportunistic scans for open ports/services
- Exploitation of unpatched vulnerabilities
- Misconfiguration abuse

## Security Goals

1. **Confidentiality**  
   Prevent unauthorized access to server and data.

2. **Integrity**  
   Protect system configuration and logs from unauthorized modification.

3. **Availability**  
   Keep essential services reachable while blocking malicious access.

4. **Accountability**  
   Ensure actions are logged for incident investigation.

## Security Principles Used

- **Least privilege**: no unnecessary sudo/users/services
- **Minimize attack surface**: install only required software
- **Defense in depth**: SSH + firewall + Fail2ban + updates + auditing
- **Secure by default**: deny inbound unless explicitly allowed
- **Recovery readiness**: backups and restore validation