# Linux Privilege Escalation – Core Concepts

A structured overview of common Linux privilege escalation vectors, misconfigurations, and defensive best practices.

---

## 1. Misplaced Passwords

One of the most common privilege escalation vectors is the exposure of sensitive information due to improper storage.

Credentials may be unintentionally exposed in:

- Bash history files  
- Backup files  
- Configuration files  
- Environment files (`.env`)  
- Git repositories  
- SSH private keys  
- Process arguments (`ps aux`)  

After gaining initial access, attackers actively search for these artifacts to escalate privileges or pivot to other accounts.

### Mitigation

- Avoid storing credentials in plaintext  
- Clear or disable history when handling sensitive data  
- Restrict access to configuration and backup files  
- Implement secure secret management practices  

---

## 2. Misconfigured Permissions

Improper file permissions can lead to unauthorized access or privilege escalation.

Examples:

- Sensitive files such as `/etc/shadow` must never be world-readable or writable.  
- Granting excessive permissions (e.g., `777`) violates the **Principle of Least Privilege**.  

If critical files are writable by unprivileged users, attackers may:

- Modify authentication mechanisms  
- Inject malicious content  
- Create backdoor accounts  

### Mitigation

- Apply the Principle of Least Privilege  
- Regularly audit file and directory permissions  
- Restrict access to system-critical files  

---

## 3. SUDO Misconfiguration

The `sudo` mechanism allows users to execute commands as another user (commonly root).

Misconfiguration occurs when:

- Users are granted unnecessary privileges  
- Commands are allowed without password (`NOPASSWD`)  
- Dangerous binaries are permitted  

### Enumeration

```bash
sudo -l
```

This command lists the allowed sudo privileges for the current user.

Attackers often rely on GTFOBins to identify privilege escalation techniques using allowed binaries.

### Mitigation

- Grant minimal sudo privileges  
- Avoid wildcard rules  
- Restrict commands precisely  
- Audit `/etc/sudoers` regularly  

---

## 4. SUID & SGID

SUID (Set User ID) and SGID (Set Group ID) are special permission bits.

- **SUID** allows execution with the file owner's privileges.  
- **SGID** allows execution with the group’s privileges.  

If a root-owned binary has SUID set and is vulnerable or misconfigured, it may allow privilege escalation.

### Enumeration

```bash
find / -perm -4000 2>/dev/null   # SUID
find / -perm -2000 2>/dev/null   # SGID
```

### Mitigation

- Remove unnecessary SUID/SGID bits  
- Periodically audit privileged binaries  
- Follow least privilege principles  

---

## 5. Cron Jobs

Cron jobs automate scheduled tasks.

If a root-owned cron job executes a script that is writable by a low-privileged user, an attacker may inject malicious commands.

Common abuse scenarios include:

- Writable scripts executed as root  
- Writable directories in execution paths  

This can lead to full privilege escalation.

### Mitigation

- Ensure scripts executed by cron are not writable by unprivileged users  
- Secure scheduled task configurations  
- Regularly audit cron entries  

---

## 6. Misconfigured Services (NFS – no_root_squash)

Improperly configured services can introduce privilege escalation risks.

A common example is NFS configured with `no_root_squash`.

When enabled:

- Remote root users retain root privileges on shared directories  
- Attackers may create malicious files with root ownership  

This can result in full system compromise.

### Mitigation

- Disable `no_root_squash`  
- Harden service configurations  
- Restrict access to trusted hosts only  

---

## 7. Kernel Exploitation

Privilege escalation may occur due to unpatched kernel vulnerabilities.

### Enumeration

```bash
uname -r
uname -a
```

Attackers typically:

- Identify the kernel version  
- Search for known local privilege escalation vulnerabilities  
- Execute compatible exploits  

### Mitigation

- Keep the system updated  
- Apply security patches regularly  
- Monitor vulnerability advisories  

---

## Final Notes

Privilege Escalation is often the result of:

- Misconfiguration  
- Excessive permissions  
- Poor credential management  
- Outdated systems  

Understanding these core concepts is essential for both offensive security (penetration testing) and defensive hardening.

Security is not only about exploiting vulnerabilities — it is also about understanding how misconfigurations create attack surfaces...
