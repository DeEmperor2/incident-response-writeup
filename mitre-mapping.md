# MITRE ATT&CK Mapping

| Technique | ID | Tactic | Evidence |
|-----------|----|--------|----------|
| Phishing — Malvertising | T1566 | Initial Access | User clicked malicious Google Authenticator ad |
| Spearphishing Link | T1566.002 | Initial Access | Malicious URL delivered via advertisement |
| User Execution | T1204.002 | Execution | User downloaded and ran malicious installer |
| Masquerading | T1036 | Defense Evasion | Payload disguised as Windows Defender update |
| Masquerade Task or Service | T1036.004 | Defense Evasion | Fake Windows Update delivery domain |
| Application Layer Protocol | T1071.001 | C2 | HTTP-based C2 communication with 5.252.153.241 |
| Ingress Tool Transfer | T1105 | C2 | 398kB file downloaded from attacker infrastructure |
| Exfiltration Over C2 Channel | T1041 | Exfiltration | 9,076 packet session indicating active data exchange |
