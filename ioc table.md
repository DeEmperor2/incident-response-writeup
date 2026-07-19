# IOC Table

| Type | Value | Tool | Verdict | Notes |
|------|-------|------|---------|-------|
| IP | 10.1.17.215 | Wireshark Endpoints | Victim | Infected workstation |
| MAC | 00:d0:b7:26:4a:74 | Wireshark DHCP | Victim | NIC of infected host |
| Hostname | DESKTOP-L8C5GSJ | Wireshark NBNS | Victim | Windows workstation |
| Domain | BLUEMOONTUESDAY | Wireshark Kerberos | Victim | AD domain |
| Username | hutchens | Wireshark Kerberos AS-REQ | Victim | Logged-in user |
| Domain | au.download.windowsupdate.com | Wireshark HTTP | Malicious | Fake Windows Update domain |
| File | am_delta_patch_1.421.1491.0_c8e6042b...exe | HTTP Export | Suspicious | Legitimate MS binary — decoy |
| Hash | c8e6042b36d8f357a8e298b6f9f2fcde561c2e02 | VirusTotal | Clean (decoy) | 0/70 detections |
| IP | 5.252.153.241 | Wireshark + VirusTotal | Malicious | C2 server — 15/91 flagged |
| ASN | AS205775 | VirusTotal | Malicious | Neon Core Network LLC — Panama |
