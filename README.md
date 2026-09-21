# Cybersecurity Week 3 - Network Security Assessment

**Intern:** Kritika Rai | **ID:** DG/SEPTEMBER/CYBER/112 | **Program:** DG Interns Hub | **Date:** 21 September 2026

## Objective
Discover hosts and exposed services with Nmap, analyze traffic with Wireshark, rate the risks, harden the target, and prove the improvement with a re-scan. Everything was done on my own authorized lab. No third-party systems were scanned.

## Lab Environment
| Role | System | IP |
|---|---|---|
| Analyst | Kali Linux VM | 10.0.0.2 |
| Target | Metasploitable2 VM | 10.0.0.4 |
| Gateway | Virtual gateway | 10.0.0.1 |

Network: 10.0.0.0/24 (mask 255.255.255.0). Diagram: `Network-Diagram/network-topology.png`

## Tools Used
Nmap 7.99, Wireshark 4.6.6, Kali Linux, VirtualBox, Metasploitable2, GitHub.

## Tasks Completed
- [x] Task 7 - Network discovery and basic Nmap scan
- [x] Task 8 - Advanced Nmap (-sV, -O, -sS, -sU, NSE vuln)
- [x] Task 9 - Wireshark capture (DNS, ICMP, ARP, TCP; HTTP via curl)
- [x] Task 10 - Nmap + Wireshark investigation
- [x] Task 11 - Traffic investigation (6 events)
- [x] Task 12 - Vulnerability assessment (11 findings)
- [x] Task 13 - Hardening (6 changes) with before/after
- [x] Task 14 - Final mini assessment (see `Week-3-Report.pdf`)

## Key Findings
| Risk | Finding |
|---|---|
| Critical | vsftpd 2.3.4 backdoor (21). NSE ran `id` and got uid=0 (root) |
| Critical | Root bind shell "ingreslock" (1524) |
| High | UnrealIRCd (6667), Java RMI default config (1099), Telnet and r-services, default logins on old web apps, exposed MySQL/PostgreSQL |
| Medium | SSLv3 POODLE on SMTP, legacy SMB/NFS, VNC/X11/old SSH and BIND |
| Low | TFTP left enabled |

23 open TCP ports were found on the target. Full list: `03-Vulnerability-Assessment/`.

## Security Improvements
1. Disabled telnet, shell, login, exec and ingreslock in `/etc/inetd.conf`
2. Disabled vsftpd in xinetd (`disable = yes`)
3. Stopped ProFTPD (2121)
4. Stopped Samba (139, 445)
5. Killed rmiregistry (1099) - temporary
6. Killed UnrealIRCd (6667) - temporary

## Before / After Results
| Issue | Before | After |
|---|---|---|
| vsftpd (21) | open | closed |
| Telnet (23) | open | closed |
| Samba (139, 445) | open | closed |
| Java RMI (1099) | open | closed |
| Root shell (1524) | open | closed |
| ProFTPD (2121) | open | closed |
| UnrealIRCd (6667) | open | closed |

8 ports confirmed closed by `nmap -sV -p 21,23,139,445,1099,1524,2121,6667 10.0.0.4`. Open TCP ports went from 23 to at most 15.

## Limits (honest notes)
- Process kills and init-script stops do not survive a reboot. inetd and xinetd edits do.
- Ports 512-514 were disabled in config but not re-scanned.
- No full-range Nmap scan was captured after hardening.
- Wireshark capture in the screenshots is about 150 s.

## Conclusion
Scanning found a large attack surface with two Critical flaws. Wireshark showed exactly what the scanner sent (SYN, SYN/ACK, RST). Six hardening changes closed 8 ports, and the re-scan proved it. Next steps: permanent service removal, a default-deny firewall and a full re-scan.

## Folder Structure
```
01-Nmap/  02-Wireshark/  03-Vulnerability-Assessment/  04-Hardening/
Network-Diagram/  Week-3-Report.pdf  Week-3-Presentation.pptx  README.md
```
