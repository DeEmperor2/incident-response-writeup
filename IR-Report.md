# Incident Response Report
**Classification:** Confidential Lab Exercise  
**Date:** July 2026  
**Analyst:** DeEmperor  
**Severity:** High  
**Status:** Resolved (Lab Environment)

---

## 1. Executive Summary
A Windows workstation (DESKTOP-L8C5GSJ) belonging to user hutchens 
was compromised through a malvertising campaign targeting users 
searching for Google Authenticator. The attacker delivered a payload 
disguised as a legitimate Windows Defender update file via a domain 
impersonating Microsoft's update infrastructure. Following infection, 
the host established persistent C2 communication with a confirmed 
malicious server (5.252.153.241), exchanging over 9,000 packets over 
an extended session. The incident was detected through network traffic 
anomaly analysis and confirmed via threat intelligence platforms.

---

## 2. Incident Details
| Field | Value |
|-------|-------|
| Date of Incident | January 22, 2025 |
| Date Detected | July 2026 (retrospective analysis) |
| Detection Method | Network traffic anomaly — Wireshark PCAP analysis |
| Incident Type | Malvertising — Malware Infection — C2 Beaconing |
| Severity | High |
| Systems Affected | DESKTOP-L8C5GSJ (10.1.17.215) |

---

## 3. Affected System Details
| Field | Value |
|-------|-------|
| IP Address | 10.1.17.215 |
| MAC Address | 00:d0:b7:26:4a:74 |
| Hostname | DESKTOP-L8C5GSJ |
| Domain | BLUEMOONTUESDAY |
| Username | hutchens |
| OS | Windows 10 |

**Detection Method:**  
Host identified via Wireshark Statistics → Endpoints. Host 10.1.17.215 
generated 39,045 of 39,427 total packets a significant anomaly 
indicating the infected machine. Hostname confirmed via NBNS traffic. 
MAC address extracted from DHCP handshake packets. Username confirmed 
via Kerberos AS-REQ packet 258, cname-string field showing "hutchens" 
in the BLUEMOONTUESDAY domain.

*Evidence: screenshots/endpoints-wireshark.png*  
*Evidence: screenshots/kerberos-username.png*

---

## 4. Attack Vector
The user searched for Google Authenticator and clicked a malicious 
advertisement. The advertisement redirected the browser to attacker 
infrastructure. A file was then delivered via a domain impersonating 
Microsoft Windows Update:

| Field | Value |
|-------|-------|
| Delivery Domain | au.download.windowsupdate.com |
| Filename | am_delta_patch_1.421.1491.0_c8e6042b36d8f357a8e298b6f9f2fcde561c2e02.exe |
| File Size | 398 kB |
| File Hash (SHA1) | c8e6042b36d8f357a8e298b6f9f2fcde561c2e02 |
| VirusTotal Result | 0/70 — legitimate Microsoft binary used as decoy |

The downloaded file is a genuine Windows Defender signature update. 
The attacker used a legitimate binary to avoid antivirus detection 
while delivering the actual payload through a separate mechanism. 
This technique using clean files as decoys is specifically 
designed to evade signature-based detection.

*Evidence: screenshots/http-export.png*  
*Evidence: screenshots/virustotal-exe.png*

---

## 5. C2 Communication
Following infection, the host established persistent communication 
with a confirmed malicious server:

| Field | Value |
|-------|-------|
| C2 IP | 5.252.153.241 |
| ASN | AS205775 — Neon Core Network LLC |
| Country | Panama |
| VirusTotal Detections | 15/91 vendors flagged malicious |
| Flagging Vendors | BitDefender, Kaspersky, ESET, Fortinet, Dr.Web, SOCRadar |
| Community Score | -59 |
| Total Packets | 9,076 packets exchanged |
| Behaviour | Active beaconing — consistent C2 pattern |

The volume of 9,076 packets between the victim and C2 server far 
exceeded all other connections in the capture, making it immediately 
identifiable as anomalous. This pattern is consistent with an 
established malware implant awaiting attacker instructions.

*Evidence: screenshots/conversations-wireshark.png*  
*Evidence: screenshots/virustotal-c2.png*

---

## 6. Detection
**Primary Detection Method:** Network traffic volume anomaly

The infected host generated 39,045 of 39,427 total packets in the 
capture 99% of all traffic. This extreme anomaly was identified 
using Wireshark Statistics → Endpoints and immediately flagged for 
investigation.

**Secondary Confirmation:** Threat intelligence lookup

The primary external IP (5.252.153.241) was submitted to VirusTotal 
and returned 15/91 malicious detections with a community score of -59, 
confirming the C2 designation.

**Tertiary Confirmation:** Endpoint telemetry

Sysmon was deployed on the host system and captured network connection 
events (Event ID 3) corroborating the network-layer findings from 
Wireshark.

*Evidence: screenshots/sysmon-events.png*

---

## 7. Attack Timeline
See [timeline.md](./timeline.md)

---

## 8. IOC Summary
See [ioc-table.md](./ioc-table.md)

---

## 9. MITRE ATT&CK Mapping
See [mitre-mapping.md](./mitre-mapping.md)

---

## 10. Containment Actions Taken (Lab)
1. Identified and isolated infected host 10.1.17.215
2. Blocked C2 IP 5.252.153.241 at network perimeter
3. Preserved PCAP evidence before remediation
4. Documented all IOCs for threat intelligence sharing

---

## 11. Recommendations
See [containment-recommendations.md](./containment-recommendations.md)

---

## 12. Lessons Learned
- Malware can evade antivirus by using legitimate binaries as decoys
- Network behaviour anomalies are often more reliable than file-based 
  detection for identifying active infections
- Volume-based traffic analysis (Wireshark Endpoints/Conversations) 
  is an effective first triage step in any investigation
- Kerberos traffic reveals usernames in plaintext in AS-REQ packets — 
  valuable for victim identification without endpoint access
- C2 beaconing produces distinctive high-volume, persistent connections 
  that stand out clearly in network captures
