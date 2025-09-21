# 🐧 LinuxJourney - Process Utilization

This file summarizes my learning from **LinuxJourney** on Linux process monitoring and utilization.  
https://linuxjourney.com/lesson/tracking-processes-top  
I practiced these on my local terminal and production environments.

---

## 🔹 Tracking processes: top
- `top` → real-time process and system monitoring.
- **CPU usage** → shows processes consuming most CPU time.
- **Memory usage** → displays RAM and swap utilization.
- **Load average** → system load over 1, 5, 15 minutes.
- `htop` → enhanced version with better interface.
- Interactive keys: `k` (kill), `r` (renice), `q` (quit).

## 🔹 lsof and fuser
- `lsof` → list open files and processes using them.
- `lsof /path/to/file` → show processes using specific file.
- `lsof -u username` → files opened by specific user.
- `lsof -p PID` → files opened by specific process.
- `fuser /path/to/file` → show PIDs using file.
- `fuser -k /path/to/file` → kill processes using file.

## 🔹 Process Threads
- **Threads** → lightweight processes sharing memory space.
- `ps -eLf` → show all threads.
- `top -H` → display individual threads.
- `pstree -p` → show process tree with PIDs.
- `/proc/PID/task/` → thread information directory.
- Threads share file descriptors and memory segments.

## 🔹 CPU Monitoring
- `vmstat 1` → CPU, memory, I/O statistics every second.
- `iostat 1` → CPU utilization and disk I/O statistics.
- `sar -u 1` → CPU usage statistics (sysstat package).
- `mpstat 1` → per-CPU statistics.
- `/proc/cpuinfo` → detailed CPU information.
- `nproc` → number of processing units available.

## 🔹 I/O Monitoring
- `iostat -x 1` → extended I/O statistics.
- `iotop` → real-time I/O usage by process.
- `lsof +D /path` → processes accessing directory.
- `/proc/PID/io` → I/O statistics for specific process.
- `pidstat -d 1` → per-process I/O statistics.
- Monitor disk read/write operations and bandwidth.

## 🔹 Memory Monitoring
- `free -h` → system memory usage (human readable).
- `vmstat 1` → virtual memory statistics.
- `pmap PID` → memory map of specific process.
- `/proc/meminfo` → detailed memory information.
- `smem` → memory usage with shared memory calculations.
- `ps aux --sort=-%mem` → processes sorted by memory usage.

## 🔹 Continuous Monitoring
- `watch command` → repeat command at intervals.
- `watch -n 2 'ps aux --sort=-%cpu'` → monitor top CPU processes.
- **System monitoring tools**: `nagios`, `zabbix`, `prometheus`.
- **Log monitoring**: `tail -f /var/log/syslog`.
- `nohup command &` → run command continuously in background.
- Set up alerts for resource thresholds.

## 🔹 Cron Jobs
- **Scheduled tasks** → automate recurring processes.
- `crontab -e` → edit user's cron jobs.
- `crontab -l` → list current cron jobs.
- **Format**: `minute hour day month weekday command`.
- `/etc/crontab` → system-wide cron jobs.
- `/var/log/cron` → cron job execution logs.
- `@reboot command` → run command at system startup.

---

📌 **Applied Insight**:  
Process utilization monitoring is critical in cybersecurity:
- **Incident response**: `top` and `ps` identify suspicious processes consuming resources.  
- **Malware detection**: Unusual CPU/memory patterns indicate potential threats.  
- **Forensic analysis**: `lsof` reveals what files malicious processes accessed.  
- **Performance monitoring**: Resource monitoring helps identify DoS attacks or crypto-mining malware.  
- **Automated monitoring**: Cron jobs enable continuous security checks and log analysis.
