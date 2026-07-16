# VIP Recovery Malware Network Forensics Investigation

## Overview

This project documents a complete network forensic investigation of the **VIP Recovery** malware infection using **Wireshark**.

The objective of this investigation was to identify the infected host, reconstruct the malware's network activity, identify indicators of compromise (IOCs), analyze attacker communications, and document the attack using industry-standard incident response techniques.

---
## Objectives

- Identify the infected workstation
- Analyze DNS requests
- Investigate HTTP communications
- Examine encrypted TLS traffic
- Identify indicators of compromise (IOCs)
- Reconstruct the attack timeline
- Map observed activity to the MITRE ATT&CK framework
- Produce a professional incident investigation report

---
## Tools Used

- Wireshark
- Malware-Traffic-Analysis.net

---
## Skills Demonstrated

- Network Forensics
- Malware Traffic Analysis
- Packet Analysis
- DNS Analysis
- HTTP Analysis
- TLS Analysis
- TCP/IP
- IOC Identification
- Incident Response
- Network Traffic Analysis
- MITRE ATT&CK Mapping
- Cyber Threat Investigation

  ---
## Investigation Methodology

The investigation followed a structured incident response process:

1. Loaded and reviewed the packet capture in Wireshark.
2. Identified the primary workstation using endpoint statistics.
3. Examined network conversations to identify external communications.
4. Analyzed DNS traffic to identify domains contacted by the infected host.
5. Investigated HTTP requests and responses.
6. Examined encrypted TLS sessions and extracted metadata.
7. Identified indicators of compromise (IOCs).
8. Reconstructed the malware's network activity.
9. Validated findings against the published investigation.
10. Documented recommendations and lessons learned.

    ---
## Investigation Findings

## Victim Workstation

- IP Address: **10.1.9.101**
- Generated the highest packet count within the capture.
- Initiated communications with multiple external hosts.

## Key Network Activity

- DNS lookups for external infrastructure.
- HTTP GET requests.
- TLS encrypted communications.
- Multiple outbound connections.
- Public IP lookup activity.

  ---
## Indicators of Compromise (IOCs)

## Domains

- firebasestorage.googleapis.com
- 1zil1.s3.cubbit.eu
- checkip.dyndns.org
- reallyfreegeoip.org
- api.telegram.org
- eraqron.shop

## IP Addresses

- 172.253.63.95
- 51.159.84.185
- 132.226.8.169
- 104.216.7.152
- 149.154.166.110
- 162.254.34.31

  ---
## MITRE ATT&CK Techniques

| Technique | Description |
|-----------|-------------|
| T1566.001 | Spearphishing Attachment |
| T1059.005 | Visual Basic |
| T1105 | Ingress Tool Transfer |
| T1071.001 | Application Layer Protocol |
| T1071.004 | DNS |
| T1041 | Exfiltration Over C2 Channel |

---
## Investigation Screenshots

## 1. Evidence Loaded

*Screenshot will be added.*

---
## 2. Victim Host Identification

*Screenshot will be added.*

---
## 3. Network Conversations

*Screenshot will be added.*

---
## 4. DNS Analysis

*Screenshot will be added.*

---
## 5. TLS Investigation

*Screenshot will be added.*

---
## 6. HTTP Requests

*Screenshot will be added.*

---
## 7. HTTP Header Analysis

*Screenshot will be added.*

---
## 8. HTTP Objects

*Screenshot will be added.*

---
## Lessons Learned

This investigation strengthened my understanding of:

- Identifying infected hosts through endpoint statistics.
- Using DNS traffic to identify attacker infrastructure.
- Following TCP conversations to understand network communications.
- Interpreting TLS metadata when payloads are encrypted.
- Examining HTTP headers to identify requested resources.
- Documenting indicators of compromise.
- Mapping observed attacker behavior to the MITRE ATT&CK framework.
- Writing a structured incident investigation report.
