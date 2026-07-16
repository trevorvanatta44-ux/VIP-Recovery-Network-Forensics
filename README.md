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

![Evidence Loaded](Screenshots/01-pcap-loaded.png)

Initial review of the packet capture confirmed that the evidence contained 5,299 packets for analysis. Wireshark was used to examine packet metadata, protocol activity, and communications between internal and external hosts.
---
## 2. Protocol Hierarchy

![Protocol Hierarchy](Screenshots/Networkprotocolhierarchy.png)

The protocol hierarchy identified the primary protocols present in the capture, including DNS, HTTP, TCP, and TLS. This overview established the types of network communications that would be investigated throughout the incident.
---
## 3. Victim Host Identification

![Victim Host](Screenshots/02-ipv4-endpoints.png)

IPv4 endpoint statistics identified **10.1.9.101** as the primary workstation involved in the investigation. This host generated the largest amount of network traffic and became the primary focus of the analysis.
---
## 4. Network Conversations

![Network Conversations](Screenshots/03-ipv4-conversations.png)

Conversation statistics revealed the external systems communicating with the infected workstation. The largest volume of traffic occurred between the victim host and **51.159.84.185**, identifying it as a high-priority communication for further investigation.
---
## 5. DNS Analysis

![DNS Analysis](Screenshots/05-dns-analysis.png)

DNS traffic identified several domains contacted by the infected workstation, including cloud storage, public IP lookup services, Telegram infrastructure, and additional external domains used during the infection.
---
## 6. HTTP Requests

![HTTP Requests](Screenshots/06-http-requests.png)

HTTP GET requests were examined to determine the resources requested by the infected host. This activity helped identify additional infrastructure contacted during the malware execution process.
---
## 7. HTTP Header Analysis

![HTTP Header](Screenshots/07-http-header-expansion.png)

Inspection of the HTTP request headers identified the destination host, request method, user agent, and requested URI. These details provided additional context regarding the workstation's outbound communications.
---
## 8. HTTP Objects

![HTTP Objects](Screenshots/httpobjectslist.png)

The HTTP Objects window was reviewed to identify files transferred over HTTP during the investigation. Reviewing transferred objects helps analysts identify downloaded content and additional artifacts associated with the malware infection.
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
