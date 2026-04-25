# Linux Server Hardening Checklist
**ISO 27001:2022 — A.8.9, A.8.1, A.8.19, A.8.2 | CIS Benchmark Level 1/2**
**Applies to:** EC2 instances, any Linux-based servers

---

## 1. Initial Configuration

| # | Control | Command / Evidence | Status | Annex A |
|---|---------|-------------------|--------|---------|
| 1.1 | OS installed from trusted/official AMI only | AWS AMI ID documented; use AWS-managed or hardened AMI | | A.8.9 |
| 1.2 | Non-essential services disabled | `systemctl list-units --type=service --state=active` | | A.8.9 |
| 1.3 | Separate filesystems for /tmp, /var, /var/log, /home | `/etc/fstab` | | A.8.9 |
| 1.4 | /tmp mounted with nodev, nosuid, noexec | `mount | grep /tmp` | | A.8.9 |
| 1.5 | System time synchronized with NTP | `timedatectl status` | | A.8.17 |
| 1.6 | Hostname properly set | `hostnamectl` | | A.8.9 |

---

## 2. Patch Management

| # | Control | Command | Status | Annex A |
|---|---------|---------|--------|---------|
| 2.1 | All packages up-to-date at deployment | `apt list --upgradable` / `yum check-update` | | A.8.8 |
| 2.2 | Automatic security updates enabled (unattended-upgrades / yum-cron) | `systemctl status unattended-upgrades` | | A.8.8 |
| 2.3 | Patch compliance monitored via AWS Systems Manager Patch Manager | SSM Patch compliance report | | A.8.8 |
| 2.4 | Critical patches applied within 7 days of release | Documented in patch management procedure | | A.8.8 |

```bash
# Ubuntu — enable unattended security upgrades
apt install unattended-upgrades
dpkg-reconfigure --priority=low unattended-upgrades
# /etc/apt/apt.conf.d/50unattended-upgrades: enable security only

# Amazon Linux — enable automatic security updates
yum install yum-cron
systemctl enable --now yum-cron
# /etc/yum/yum-cron.conf: apply_updates = yes

# Check patch compliance via SSM
aws ssm describe-instance-patch-states --instance-ids [i-xxxxxxxxx]
```

---

## 3. SSH Hardening

| # | Control | Config Location | Value | Status | Annex A |
|---|---------|----------------|-------|--------|---------|
| 3.1 | SSH root login disabled | `/etc/ssh/sshd_config` | `PermitRootLogin no` | | A.8.2 |
| 3.2 | SSH password authentication disabled | `/etc/ssh/sshd_config` | `PasswordAuthentication no` | | A.8.5 |
| 3.3 | SSH uses protocol 2 only | `/etc/ssh/sshd_config` | `Protocol 2` | | A.8.5 |
| 3.4 | SSH allowed users restricted | `/etc/ssh/sshd_config` | `AllowUsers [list]` | | A.8.2 |
| 3.5 | SSH key type: Ed25519 or RSA 4096 | SSH key generation | `ssh-keygen -t ed25519` | | A.8.24 |
| 3.6 | SSH idle timeout | `/etc/ssh/sshd_config` | `ClientAliveInterval 300`, `ClientAliveCountMax 3` | | A.8.5 |
| 3.7 | SSH port non-standard (optional) or access only via SSM | Security group; SSM preference | No inbound 22 from 0.0.0.0/0 | | A.8.20 |
| 3.8 | SSH MaxAuthTries set | `/etc/ssh/sshd_config` | `MaxAuthTries 4` | | A.8.5 |
| 3.9 | SSH banner configured | `/etc/ssh/sshd_config` | `Banner /etc/issue.net` | | A.8.9 |
| 3.10 | SSH login attempts logged | `rsyslog` config | `AUTH.*` to `/var/log/auth.log` | | A.8.15 |

```bash
# Preferred: use AWS SSM Session Manager instead of SSH
# Eliminates need for open port 22 entirely
# Requirements: SSM Agent installed, IAM role with SSM permissions, VPC endpoints or NAT

# SSM session start
aws ssm start-session --target i-xxxxxxxxxxxxxxxxx

# If SSH required — harden config
cat >> /etc/ssh/sshd_config << 'EOF'
PermitRootLogin no
PasswordAuthentication no
Protocol 2
MaxAuthTries 4
ClientAliveInterval 300
ClientAliveCountMax 3
AllowAgentForwarding no
AllowTcpForwarding no
X11Forwarding no
EOF
systemctl restart sshd
```

---

## 4. User Account Management

| # | Control | Command | Status | Annex A |
|---|---------|---------|--------|---------|
| 4.1 | No unnecessary user accounts | `cat /etc/passwd \| awk -F: '$3 >= 1000'` | | A.5.18 |
| 4.2 | Root account locked (use sudo) | `passwd -l root` | | A.8.2 |
| 4.3 | All users with UID 0 reviewed | `awk -F: '$3 == 0' /etc/passwd` | Should only be root | | A.8.2 |
| 4.4 | Default accounts disabled (nobody, games, etc.) | `passwd -l [account]` | | A.5.18 |
| 4.5 | sudo access limited to named users/groups | `/etc/sudoers` or `/etc/sudoers.d/` | No `NOPASSWD` without justification | | A.8.2 |
| 4.6 | Password aging enforced for any password-auth accounts | `/etc/login.defs`: `PASS_MAX_DAYS 90` | | A.5.17 |
| 4.7 | umask set to 027 or stricter | `/etc/profile`, `/etc/bashrc` | `umask 027` | | A.8.3 |

---

## 5. Filesystem Permissions

| # | Control | Command | Status | Annex A |
|---|---------|---------|--------|---------|
| 5.1 | World-writable files audited | `find / -xdev -type f -perm -0002` | | A.8.3 |
| 5.2 | No SUID/SGID files other than approved list | `find / -xdev \( -perm -4000 -o -perm -2000 \) -type f` | | A.8.3 |
| 5.3 | /etc/shadow permissions: 000 | `stat /etc/shadow` | | A.8.3 |
| 5.4 | /etc/passwd permissions: 644 | `stat /etc/passwd` | | A.8.3 |
| 5.5 | Core dumps disabled | `/etc/security/limits.conf`: `* hard core 0` | | A.8.9 |

---

## 6. Logging and Auditing

| # | Control | Config | Status | Annex A |
|---|---------|--------|--------|---------|
| 6.1 | auditd installed and running | `systemctl status auditd` | | A.8.15 |
| 6.2 | Audit rules: log all privileged commands | `/etc/audit/audit.rules` | | A.8.15 |
| 6.3 | Audit rules: log file permission changes | `/etc/audit/audit.rules` | | A.8.15 |
| 6.4 | Audit rules: log user/group changes | `/etc/audit/audit.rules` | | A.8.15 |
| 6.5 | Logs shipped to central SIEM/CloudWatch | CloudWatch agent installed and configured | | A.8.15 |
| 6.6 | syslog/rsyslog configured and running | `systemctl status rsyslog` | | A.8.15 |
| 6.7 | Log rotation configured | `/etc/logrotate.conf` | | A.8.15 |

```bash
# Install and configure auditd
apt install auditd audispd-plugins  # Ubuntu
yum install audit                    # Amazon Linux

# Key audit rules (/etc/audit/rules.d/hardening.rules)
cat > /etc/audit/rules.d/hardening.rules << 'EOF'
# Log all privileged command executions
-a always,exit -F arch=b64 -S execve -F euid=0 -k privileged
# Log file permission changes
-a always,exit -F arch=b64 -S chmod,fchmod,chown,lchown -k perm_mod
# Log user/group changes
-w /etc/group -p wa -k identity
-w /etc/passwd -p wa -k identity
-w /etc/shadow -p wa -k identity
# Log sudoers changes
-w /etc/sudoers -p wa -k sudoers
EOF
service auditd restart
```

---

## 7. Network Configuration

| # | Control | Command | Status | Annex A |
|---|---------|---------|--------|---------|
| 7.1 | IPv6 disabled if not used | `/etc/sysctl.conf`: `net.ipv6.conf.all.disable_ipv6=1` | | A.8.20 |
| 7.2 | IP forwarding disabled (unless gateway/NAT) | `sysctl net.ipv4.ip_forward` should be 0 | | A.8.20 |
| 7.3 | ICMP redirects disabled | `net.ipv4.conf.all.accept_redirects=0` | | A.8.20 |
| 7.4 | SYN flood protection enabled | `net.ipv4.tcp_syncookies=1` | | A.8.20 |
| 7.5 | Firewall enabled (iptables / ufw / nftables) | `ufw status` / `iptables -L` | | A.8.20 |
| 7.6 | Only required ports open in host firewall | Review with `ss -tlnp` | | A.8.20 |

---

## 8. File Integrity Monitoring

| # | Control | Tool | Status | Annex A |
|---|---------|------|--------|---------|
| 8.1 | File integrity monitoring installed (AIDE, Tripwire, or AWS FIM) | `aide --check` | | A.8.9 |
| 8.2 | FIM baseline taken after hardening | `aide --init` | | A.8.9 |
| 8.3 | FIM alerts on unauthorized changes | Alert sent to SIEM | | A.8.16 |

---

## Hardening Score

| Section | Controls | Passing | % |
|---------|---------|---------|---|
| Initial Config | 6 | | |
| Patch Management | 4 | | |
| SSH | 10 | | |
| User Accounts | 7 | | |
| Filesystem | 5 | | |
| Logging | 7 | | |
| Network | 6 | | |
| FIM | 3 | | |
| **Total** | **48** | | |
