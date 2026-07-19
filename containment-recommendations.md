# Containment and Remediation Recommendations

## Immediate Actions
1. **Isolate the host** remove DESKTOP-L8C5GSJ from the network immediately
2. **Block C2 IP** add 5.252.153.241 to firewall blocklist
3. **Block delivery domain** blacklist au.download.windowsupdate.com at DNS level
4. **Preserve evidence** image the disk before any remediation
5. **Reset credentials** force password reset for hutchens account and all domain accounts
6. **Check lateral movement** review logs for any connections from 10.1.17.215 to other hosts

## Short Term (within 48 hours)
1. **Scan all hosts** for connections to 5.252.153.241 in firewall/proxy logs
2. **Deploy EDR** on all endpoints behavioural detection catches C2 beaconing 
   even when files evade AV
3. **Review DNS logs** for other hosts resolving au.download.windowsupdate.com
4. **Submit IOCs** to threat intelligence platforms for broader blocking

## Long Term
1. **Ad blocking at network level** DNS-based ad blocking removes malvertising vector
2. **Web proxy with URL categorisation** blocks newly registered or suspicious domains
3. **User awareness training** train users to download software only from official 
   vendor websites, never via search advertisements
4. **MFA enforcement** prevents account takeover even if credentials are stolen
5. **Network traffic monitoring** anomalous outbound volume should trigger 
   automated SIEM alerts
6. **Application whitelisting** prevents execution of unauthorised binaries

## Detection Rules (Splunk)
To detect similar C2 beaconing in future:
**Query 1 — Direct C2 IP detection:**
index=network dest_ip="5.252.153.241"
| stats count by src_ip
| where count > 100

**Query 2 High volume outbound traffic anomaly:**
index=network bytes_out > 1000000
| stats count by dest_ip
| sort -count

**Query 3 Detect beaconing pattern (regular intervals):**
index=network
| bucket _time span=1m
| stats count by _time dest_ip
| eventstats stdev(count) as stdev avg(count) as avg by dest_ip
| where stdev < 2 AND avg > 10

**Query 4 New external connections from infected host:**
index=network src_ip="10.1.17.215"
| stats count by dest_ip
| where NOT cidrmatch("10.0.0.0/8", dest_ip)
| sort -count

## Defensive Layer Summary
| Layer | Control | Status |
|-------|---------|--------|
| Network | Ad blocking | Recommended |
| Network | Web proxy | Recommended |
| Network | Firewall C2 block | Implemented (lab) |
| Endpoint | EDR deployment | Recommended |
| Endpoint | AV (signature) | Insufficient alone |
| Identity | MFA | Recommended |
| User | Awareness training | Recommended |
