# Hardening Log (Task 13) - target 10.0.0.4

| # | Issue | Action | Command |
|---|---|---|---|
| H1 | inetd services (telnet, shell, login, exec, ingreslock) | Commented out | `sed -i '/^telnet/s/^/#/' /etc/inetd.conf` (same for the others), `/etc/init.d/openbsd-inetd restart` |
| H2 | vsftpd backdoor (21) | Disabled in xinetd | `sed -i '/}/i\ disable=yes' /etc/xinetd.d/vsftpd`, `/etc/init.d/xinetd restart` |
| H3 | ProFTPD (2121) | Stopped | `/etc/init.d/proftpd stop` |
| H4 | UnrealIRCd (6667) | Killed PID 4663 (temporary) | `kill -9 4663` |
| H5 | Java RMI (1099) | Killed PID 4655 (temporary) | `kill -9 4655` |
| H6 | Samba (139, 445) | Stopped | `/etc/init.d/samba stop` |

Before: `Before/` (inetd.conf enabled, full scans). After: `After/` (config edits, netstat, re-scan with 8 ports closed).

Note: the first attempt `/etc/init.d/vsftpd stop` failed (service runs under xinetd). netstat showed port 21 still listening, which led to fix H2.
