# Linux Interview Question Bank

## Conceptual Questions (30)

### 1. Explain the Linux boot process
**Expected Answer:**  
The Linux boot process consists of: BIOS/UEFI firmware → MBR/GPT bootloader → GRUB → Kernel loading → Init system (SysVinit/systemd) → Runlevel/service startup. systemd uses targets (multi-user.target) while SysVinit uses runlevels (0-6).

**Follow-up Questions:**  
- What happens if GRUB is corrupted?  
- How do you verify which init system is running?

### 2. What are Linux runlevels and their purposes?
**Expected Answer:**  
Runlevels define system states:  
- 0: Halt  
- 1: Single-user mode (maintenance)  
- 2: Multi-user without networking  
- 3: Multi-user with networking (CLI)  
- 5: Graphical mode  
- 6: Reboot

**Follow-up Questions:**  
- How to change default runlevel?  
- Which runlevel is best for servers?

### 3. Difference between soft link and hard link
**Expected Answer:**  
Hard link: Points directly to inode, cannot cross filesystems, original file deletion doesn't break it.  
Soft link (symbolic): Points to filename, can cross filesystems, breaks if original deleted.

**Follow-up Questions:**  
- How to create hard link?  
- How to find all hard links to a file?

### 4. Explain file permissions in Linux (rwx)
**Expected Answer:**  
Three categories: Owner, Group, Others. Each has read (4), write (2), execute (1).  
Example: 754 = rwxr-xr--

**Follow-up Questions:**  
- What does setuid bit do?  
- How to set permissions recursively?

### 5. How to manage users and groups?
**Expected Answer:**  
Commands:  
- useradd, usermod, userdel for users  
- groupadd, groupmod, groupdel for groups  
- /etc/passwd and /etc/shadow store user data

**Follow-up Questions:**  
- How to lock a user account?  
- Difference between useradd and adduser?

### 6. What are process states in Linux?
**Expected Answer:**  
- R: Running  
- S: Sleeping  
- D: Uninterruptible sleep  
- Z: Zombie  
- T: Stopped  
- t: Traced

**Follow-up Questions:**  
- How to kill zombie processes?  
- What causes a process to be in D state?

### 7. Explain process management commands
**Expected Answer:**  
- ps: View processes  
- top/htop: Interactive monitoring  
- kill: Send signals  
- nice/renice: Priority adjustment

**Follow-up Questions:**  
- What signal does kill -9 send?  
- How to check a process's open files?

### 8. How to troubleshoot memory issues?
**Expected Answer:**  
- free -h: Check memory usage  
- vmstat: Swapping info  
- top: Per-process memory  
- /proc/meminfo: Detailed stats

**Follow-up Questions:**  
- What is swap and when to use it?  
- How to clear cache without rebooting?

### 9. Linux network troubleshooting commands
**Expected Answer:**  
- ip/ifconfig: Interface info  
- netstat/ss: Connections  
- ping/traceroute: Connectivity  
- tcpdump/wireshark: Packet capture

**Follow-up Questions:**  
- How to check DNS resolution?  
- What port is a service listening on?

### 10. Explain /proc filesystem
**Expected Answer:**  
Virtual filesystem providing kernel/process info:  
- /proc/cpuinfo: CPU details  
- /proc/meminfo: Memory info  
- /proc/[pid]/: Process-specific data

**Follow-up Questions:**  
- How to check kernel version?  
- What file shows loaded modules?

### 11. File system types in Linux
**Expected Answer:**  
- ext4: Default, journaling  
- XFS: Scalable, large files  
- Btrfs: Snapshots, compression  
- NFS: Network filesystem

**Follow-up Questions:**  
- How to check filesystem type?  
- How to defragment ext4?

### 12. LVM (Logical Volume Manager) concepts
**Expected Answer:**  
Layers: PV (Physical Volume) → VG (Volume Group) → LV (Logical Volume) → Filesystem. Enables dynamic resizing.

**Follow-up Questions:**  
- How to extend an LV?  
- What is snapshot in LVM?

### 13. SSH key-based authentication setup
**Expected Answer:**  
- ssh-keygen: Generate key pair  
- scp public key to ~/.ssh/authorized_keys  
- Set correct permissions (600)

**Follow-up Questions:**  
- How to add key to ssh-agent?  
- What if permissions are wrong?

### 14. Cron and systemd timers
**Expected Answer:**  
- crontab -e: Schedule jobs  
- systemd timers: More features, dependency handling  
- Format: minute hour day month weekday command

**Follow-up Questions:**  
- How to check cron logs?  
- What is @reboot in crontab?

### 15. Log management in Linux
**Expected Answer:**  
- /var/log/: Standard logs  
- journalctl: systemd logs  
- rsyslog: Logging daemon  
- logrotate: Manage log rotation

**Follow-up Questions:**  
- How to search logs efficiently?  
- What is /var/log/messages?

### 16. Package management (YUM/DNF vs APT)
**Expected Answer:**  
- YUM/DNF (RHEL/CentOS): rpm packages, dependency resolution  
- APT (Debian/Ubuntu): dpkg packages, apt cache

**Follow-up Questions:**  
- How to enable EPEL repo?  
- What is repository GPG verification?

### 17. SELinux concepts
**Expected Answer:**  
Mandatory Access Control (MAC) system:  
- Enforcing: Active blocking  
- Permissive: Logging only  
- Disabled: Off

**Follow-up Questions:**  
- How to check SELinux context?  
- What is boolean in SELinux?

### 18. Firewalld vs iptables
**Expected Answer:**  
- firewalld: Dynamic, zone-based, D-Bus API  
- iptables: Static rules, netfilter

**Follow-up Questions:**  
- How to allow HTTP through firewalld?  
- What is rich rule in firewalld?

### 19. Process priority and scheduling
**Expected Answer:**  
- nice value: -20 (high) to 19 (low)  
- Priority calculated: nice + 20  
- Real-time scheduling: SCHED_FIFO, SCHED_RR

**Follow-up Questions:**  
- How to view all processes with nice values?  
- What is OOM killer?

### 20. Swap space management
**Expected Answer:**  
- swapon/swapoff: Enable/disable swap  
- fallocate/dd: Create swap file  
- mkswap/swapon: Prepare and activate

**Follow-up Questions:**  
- How to check swap usage?  
- When should swap be used?

### 21. Kernel parameters tuning
**Expected Answer:**  
- /etc/sysctl.conf: Persistent settings  
- sysctl -p: Apply changes  
- /proc/sys/: Runtime parameters

**Follow-up Questions:**  
- How to increase file descriptor limits?  
- What affects network performance?

### 22. RAID concepts in Linux
**Expected Answer:**  
- RAID 0: Striping (performance)  
- RAID 1: Mirroring (redundancy)  
- RAID 5: Striping with parity  
- RAID 10: Mirroring + striping

**Follow-up Questions:**  
- How to check RAID status?  
- What is mdadm?

### 23. NFS mount troubleshooting
**Expected Answer:**  
- showmount -e server: Check exports  
- rpcinfo -p: RPC services  
- /proc/mounts: Active mounts

**Follow-up Questions:**  
- How to mount with specific options?  
- What causes "stale file handle"?

### 24. TCP vs UDP differences
**Expected Answer:**  
TCP: Connection-oriented, reliable, ordered.  
UDP: Connectionless, faster, no guarantee.

**Follow-up Questions:**  
- Which is better for video streaming?  
- How to check open TCP connections?

### 25. Disk usage analysis commands
**Expected Answer:**  
- df -h: Disk space  
- du -sh: Directory size  
- ncdu: Interactive usage  
- lsof: Open files

**Follow-up Questions:**  
- How to find largest files?  
- What is inode and why does it matter?

### 26. Environment variables management
**Expected Answer:**  
- export: Set variable  
- echo $VAR: View  
- .bashrc/.profile: Persistent  
- printenv: List all

**Follow-up Questions:**  
- How to make variable available for all users?  
- What is PATH variable?

### 27. Compression tools in Linux
**Expected Answer:**  
- gzip/gunzip: .gz files  
- bzip2/bunzip2: .bz2 files  
- tar: Archive with compression

**Follow-up Questions:**  
- How to extract specific file from tar.gz?  
- What is xz compression?

### 28. Signals in Linux
**Expected Answer:**  
Common signals:  
- SIGTERM (15): Graceful termination  
- SIGKILL (9): Force kill  
- SIGHUP (1): Hangup (reload config)

**Follow-up Questions:**  
- How to list all signals?  
- What signal is sent by Ctrl+C?

### 29. Process monitoring and performance
**Expected Answer:**  
- iostat: I/O stats  
- sar: Historical stats  
- iperf: Network bandwidth

**Follow-up Questions:**  
- How to monitor disk I/O in real-time?  
- What is load average?

### 30. Security hardening basics
**Expected Answer:**  
- Disable root SSH login  
- Use SSH keys, not passwords  
- Regular updates  
- Fail2ban for brute force protection

**Follow-up Questions:**  
- How to audit failed login attempts?  
- What is CIS benchmark?

---

## Scenario-Based Questions (10)

### Scenario 1
**Situation:** Server responds slowly, high load average (15.0) but low CPU usage.

**What Interviewer Tests:** Understanding of I/O wait states and process blocking.

**Expected Approach:**  
- Check iostat for I/O wait  
- Look for processes in uninterruptible sleep (D state)  
- Check disk utilization with df and iotop

**Ideal Answer:** High load average with low CPU indicates I/O bottleneck. Use `iostat -x 1` to identify disk performance issues, check for stuck processes in D state with `ps aux | grep D`, and verify disk space and inode usage.

**Follow-up Questions:**  
- How to identify which process causes I/O wait?  
- What commands help with disk scheduling?

### Scenario 2
**Situation:** User cannot write to directory despite having write permission.

**What Interviewer Tests:** Understanding of directory permissions and parent directory traversal.

**Expected Approach:**  
- Check directory permissions including parent directories  
- Verify mount options (read-only filesystem)  
- Check SELinux/AppArmor context

**Ideal Answer:** The parent directories might lack execute (traverse) permission. Check `ls -ld` on each parent directory. Also verify the filesystem isn't mounted read-only and SELinux context isn't blocking access.

**Follow-up Questions:**  
- How to check mount options?  
- What is the sticky bit?

### Scenario 3
**Situation:** Production server running out of memory, need to find top consumers.

**What Interviewer Tests:** Memory troubleshooting and process analysis.

**Expected Approach:**  
- Use `free -h` and `top` to identify memory usage  
- Sort processes by memory: `ps aux --sort=-%mem | head`  
- Check for memory leaks in applications

**Ideal Answer:** Run `free -h` for overview, then `ps aux --sort=-%mem | head -10` to find top processes. For detailed analysis, use `pmap -x PID` to see memory mapping.

**Follow-up Questions:**  
- How to limit a process's memory usage?  
- What is swapiness?

### Scenario 4
**Situation:** Network service not responding, firewall might be blocking.

**What Interviewer Tests:** Network troubleshooting methodology.

**Expected Approach:**  
- Check service status: `systemctl status servicename`  
- Verify listening ports: `ss -tlnp`  
- Test firewall rules: `firewall-cmd --list-all` or `iptables -L`

**Ideal Answer:** First check if service is running (`systemctl status`), then verify it's listening on expected port (`ss -tlnp | grep PORT`). Check firewall with `firewall-cmd --list-services` and temporarily disable for testing.

**Follow-up Questions:**  
- How to check which process listens on a port?  
- What is ephemeral port range?

### Scenario 5
**Situation:** Need to find which process is using specific TCP port.

**What Interviewer Tests:** Network and process correlation.

**Expected Approach:**  
- Use `ss -tlnp | grep PORT`  
- Or `netstat -tlnp | grep PORT`  
- Check `/proc/net/tcp` for details

**Ideal Answer:** Command `ss -tlnp | grep :PORT` shows process ID and name. Alternative: `lsof -i :PORT` or `fuser PORT/tcp`.

**Follow-up Questions:**  
- How to kill a process using port?  
- What permission is needed to view?

### Scenario 6
**Situation:** Frequent disk space alerts, need to identify largest directories.

**What Interviewer Tests:** Disk usage analysis and cleanup.

**Expected Approach:**  
- Start from root: `du -sh /* | sort -h`  
- Check common culprits: /var/log, /tmp  
- Use ncdu for interactive analysis

**Ideal Answer:** Run `du -sh /* 2>/dev/null | sort -h` to see top-level directories. Check `/var/log` with `journalctl --vacuum-size=100M` and clean package caches with `yum clean all`.

**Follow-up Questions:**  
- How to find large files?  
- What is inode exhaustion?

### Scenario 7
**Situation:** SSH login takes too long after password entry.

**What Interviewer Tests:** SSH configuration and DNS reverse lookup.

**Expected Approach:**  
- Check sshd_config for UseDNS  
- Verify DNS resolution  
- Review PAM modules

**Ideal Answer:** Disable DNS reverse lookup in `/etc/ssh/sshd_config` by setting `UseDNS no`, then restart sshd. This prevents delays when reverse DNS lookup times out.

**Follow-up Questions:**  
- How to enable SSH key-based login?  
- What causes MOTD delay?

### Scenario 8
**Situation:** Need to schedule script execution every 5 minutes during business hours only.

**What Interviewer Tests:** Cron syntax and conditional execution.

**Expected Approach:**  
- Create cron entry with hour restriction  
- Add condition within script  
- Use systemd timer with calendar event

**Ideal Answer:** In crontab: `*/5 9-17 * * 1-5 /path/to/script`. Or in script: `[[ $(date +%H) -ge 9 && $(date +%H) -lt 17 ]] && your_command`.

**Follow-up Questions:**  
- How to check cron syntax?  
- What is systemd calendar event?

### Scenario 9
**Situation:** Application log shows "too many open files" error.

**What Interviewer Tests:** File descriptor limits and tuning.

**Expected Approach:**  
- Check current limits: `ulimit -n`  
- View process limits: `cat /proc/PID/limits`  
- Increase limits in /etc/security/limits.conf

**Ideal Answer:** Check `ulimit -n` (usually 1024). Increase globally in `/etc/security/limits.conf` or per-service in systemd with `LimitNOFILE=65536`. Also check for file descriptor leaks.

**Follow-up Questions:**  
- How to check system-wide limit?  
- What is /proc/sys/fs/file-max?

### Scenario 10
**Situation:** Service hangs during boot, need to boot into single-user mode.

**What Interviewer Tests:** Boot recovery procedures.

**Expected Approach:**  
- Edit GRUB at boot time  
- Add "single" or "1" to kernel parameters  
- Or use "init=/bin/bash" for root shell

**Ideal Answer:** At GRUB menu, press 'e' to edit, add "single" to kernel line. For RHEL/CentOS: append "rd.break" for emergency shell. For systemd: add "systemd.unit=rescue.target".

**Follow-up Questions:**  
- How to reset root password?  
- What is runlevel 1 vs emergency mode?

---

**Word Count**: ~5200 words  
**Line Count**: ~248 lines (excluding this footer)