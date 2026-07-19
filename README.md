# Incident Response Writeup — Malvertising Campaign

## Scenario
A Windows workstation was compromised after a user searched for 
Google Authenticator and clicked a malicious advertisement. This 
report documents the full incident response process detection, 
investigation, containment and recommendations.

## Skills Demonstrated
- Network traffic analysis (Wireshark)
- Host identification via DHCP, NBNS, Kerberos
- C2 server identification and threat intelligence lookup
- Malware delivery mechanism analysis
- MITRE ATT&CK technique mapping
- Incident timeline reconstruction
- Sysmon log analysis
- Professional IR report writing

## Tools Used
- Wireshark — packet capture analysis
- Sysmon — endpoint telemetry
- VirusTotal — IOC reputation
- Nmap — network reconnaissance
- Event Viewer — Windows log analysis

## Key Findings
| Item | Detail |
|------|--------|
| Infected Host | 10.1.17.215 (DESKTOP-L8C5GSJ) |
| User | BLUEMOONTUESDAY\hutchens |
| Attack Type | Malvertising → malware download → C2 beaconing |
| C2 Server | 5.252.153.241 (15/91 vendors — malicious) |
| Delivery Domain | au.download.windowsupdate.com |

## Files
- [IR Report](./IR-Report.md)
- [Attack Timeline](./timeline.md)
- [IOC Table](./ioc-table.md)
- [MITRE ATT&CK Mapping](./mitre-mapping.md)
- [Containment Recommendations](./containment-recommendations.md)
- [Screenshots](./screenshots/)
