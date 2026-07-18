# VIP Recovery Malware Network Forensics Investigation

## Overview

This project documents a complete network forensic investigation of the **VIP Recovery** malware infection using **Wireshark**.

The objective of this investigation was to identify the infected host, reconstruct the malware's network activity, identify indicators of compromise (IOCs), analyze attacker communications, and document the attack using a structured Digital Forensics and Incident Response (DFIR) network forensics methodology, with observed adversary behaviors mapped to the MITRE ATT&CK framework.

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
- MITRE ATT&CK Framework

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
This investigation followed a structured Digital Forensics and Incident Response (DFIR) network forensics methodology. The investigation began with evidence acquisition and packet capture validation before identifying the compromised host through endpoint statistics. Network traffic was then analyzed across DNS, HTTP, SMTP, TLS, and TCP protocols to identify attacker infrastructure, reconstruct the malware's communication patterns, extract indicators of compromise (IOCs), and develop an attack timeline. Observed adversary behaviors were mapped to the MITRE ATT&CK framework, and all findings were documented in a formal incident investigation report.

---
## Detection & Analysis Workflow

1. Validated the packet capture and confirmed evidence integrity.
2. Identified the compromised workstation using endpoint statistics.
3. Profiled network protocols to establish baseline communications.
4. Analyzed DNS requests to identify external infrastructure.
5. Examined HTTP communications and request headers.
6. Reviewed SMTP traffic to identify data exfiltration activity.
7. Inspected TLS sessions and metadata for encrypted communications.
8. Extracted indicators of compromise (IOCs), including domains and IP addresses.
9. Reconstructed the malware's network activity and attack timeline.
10. Mapped observed adversary behavior to the MITRE ATT&CK framework.
11. Documented findings in a structured incident investigation report.

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
## 9. SMTP Data Exfiltration

![SMTP Traffic](Screenshots/SMTP.png)

Analysis of traffic over TCP port 587 identified unencrypted SMTP communications between the infected workstation and **162.254.34.31**. The activity was associated with the `eraqron.shop` infrastructure and was used to transmit victim data through email.

The identified sender and recipient addresses were:

- **Sender:** `rejump@eraqron.shop`
- **Recipient:** `jump@eraqron.shop`

Because the SMTP session was not encrypted, Wireshark could inspect the email protocol activity directly. This provided evidence of post-infection data exfiltration from the compromised workstation.
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
