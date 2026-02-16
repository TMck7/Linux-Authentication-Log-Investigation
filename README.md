# Linux-Authentication-Log-Investigation

Investigation of failed SSH authentication attempts on an Ubuntu 24.04 server within a personal SOC lab environment.

---

## Objective

Simulate suspicious SSH login activity and perform structured authentication log analysis to identify:

- Failed login attempts
- Source IP address
- Attempt frequency
- Successful authentication event

---

## Environment

- Ubuntu Server 24.04 LTS
- VirtualBox NAT networking
- SSH accessed via PuTTY
- Log file analyzed: /var/log/auth.log

---

## Attack Simulation

Using PuTTY, multiple incorrect password attempts were intentionally entered against the SSH service before a successful login was performed.

This simulated brute-force style authentication behavior.

---

## Evidence Collection

### 1. Failed SSH Authentication Events

Command used:

```bash
sudo grep "sshd" /var/log/auth.log | grep "Failed password"
```

Findings:
Multiple failed password attempts were recorded for user `tmck7`.

---

### 2. Source IP Extraction

Command used:

```bash
sudo grep "sshd" /var/log/auth.log | grep "Failed password" | awk '{print $9}'
```

Findings:
All failed attempts originated from:

10.0.2.2

---

### 3. Failed Attempt Count by IP

Command used:

```bash
sudo grep "sshd" /var/log/auth.log | grep "Failed password" | awk '{print $9}' | sort | uniq -c | sort -nr
```

Findings:
The majority of failed attempts were associated with IP address 10.0.2.2.

Note:
Ubuntu compresses repeated log entries using the format:
"message repeated X times"

This affects simple field extraction and demonstrates real-world log parsing considerations.

---

### 4. Successful Authentication Event

Command used:

```bash
sudo grep "Accepted password" /var/log/auth.log
```

Findings:
A successful login occurred after multiple failed attempts from the same IP address.

---

## Analysis Summary

- Repeated failed SSH login attempts were observed.
- All attempts originated from 10.0.2.2 (host machine under NAT).
- The pattern resembles brute-force authentication behavior.
- A successful login followed the failed attempts.

This investigation demonstrates:

- SSH log analysis
- Indicator extraction (source IP)
- Frequency analysis
- Understanding of compressed log entries
- Basic incident-style documentation

---

## Skills Demonstrated

- Linux authentication log analysis
- Use of grep and awk for log parsing
- Frequency counting with sort and uniq
- Identification of suspicious login behavior
- Structured SOC-style documentation

---

## Next Phase

Expand analysis to include:

- Detection scripting
- Automated brute-force detection
- Log alert simulation
