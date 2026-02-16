# Linux-Authentication-Log-Investigation

Structured analysis of SSH authentication failures on an Ubuntu 24.04 server within a self-deployed SOC lab environment.

---

## Objective

Demonstrate the ability to identify, extract, and analyze failed SSH authentication attempts using native Linux log files and command-line tooling.

---

## Environment

- Ubuntu Server 24.04 LTS
- Oracle VirtualBox (NAT networking)
- SSH access via PuTTY
- Log file analyzed: /var/log/auth.log

---

## Event Generation

Repeated incorrect SSH password attempts were performed against the server prior to a successful login. This generated authentication failure entries for investigation within `/var/log/auth.log`.

---

## Investigation & Findings

### Failed Authentication Events

Command executed:

sudo grep "sshd" /var/log/auth.log | grep "Failed password"


Multiple failed authentication attempts were identified for user `tmck7`.

---

### Source IP Identification

Command executed:

sudo grep "sshd" /var/log/auth.log | grep "Failed password" | awk '{print $9}'


All failed attempts originated from:

10.0.2.2


---

### Failed Attempt Frequency Analysis

Command executed:

sudo grep "sshd" /var/log/auth.log | grep "Failed password" | awk '{print $9}' | sort | uniq -c | sort -nr


The majority of failed authentication attempts were associated with IP address `10.0.2.2`.

Note:
Ubuntu compresses repeated log entries using the format `message repeated X times`, which impacts simple field extraction and reflects realistic log parsing considerations.

---

### Successful Authentication Event

Command executed:

sudo grep "Accepted password" /var/log/auth.log


A successful SSH login occurred after multiple failed attempts from the same source IP.

---

## Analytical Summary

- Repeated SSH authentication failures were detected.
- All attempts originated from 10.0.2.2 (host system under NAT configuration).
- The activity pattern is consistent with brute-force style authentication behavior.
- A successful login followed the failed attempts.

---

## Technical Skills Demonstrated

- Linux authentication log analysis
- Log filtering using `grep`
- Field extraction using `awk`
- Frequency analysis using `sort` and `uniq`
- Interpretation of compressed log entries
- Structured incident-style documentation

---

## Future Enhancements

- Develop automated brute-force detection script
- Implement threshold-based alert logic
- Expand analysis to additional log sources
