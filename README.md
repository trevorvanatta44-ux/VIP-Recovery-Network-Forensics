# VIP Recovery Malware Network Forensics Investigation

## Executive Summary

This project documents a network forensic investigation of the VIP Recovery malware-related packet capture (PCAP) provided by Malware-Traffic-Analysis.net. Using Wireshark, the PCAP was analyzed to identify the compromised workstation, analyze DNS, HTTP, TLS, and SMTP communications observed in the capture, extract indicators of compromise (IOCs), reconstruct the observed communication sequence, and map network behaviors to the MITRE ATT&CK framework.

## Background

The Malware-Traffic-Analysis.net case scenario describes an incident in which a phishing email containing a malicious ZIP archive was delivered to a victim. According to the scenario, extracting the archive executed a Visual Basic Script (VBS), resulting in malicious network activity that was recorded in a packet capture (PCAP). The PCAP served as the primary source of forensic evidence analyzed during this investigation.

Although the case scenario provided background information about the suspected infection chain, the findings presented in this report are based solely on forensic analysis of the PCAP using Wireshark. All documented indicators of compromise (IOCs), observed network communications, and investigation conclusions were derived from evidence contained within the capture.

## Objectives

- Identify the compromised workstation
- Analyze DNS requests
- Investigate HTTP communications
- Examine encrypted TLS traffic
- Identify indicators of compromise (IOCs)
- Reconstruct the observed communication sequence
- Map observed network behaviors to the MITRE ATT&CK framework
- Document the investigation findings


## Tools Used

- Wireshark
- MITRE ATT&CK Framework

## Data Source

-Malware-Traffic-Analysis.net (VIP recovery PCAP)


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

## Investigation Methodology
This investigation followed a structured Digital Forensics and Incident Response (DFIR) network forensics methodology. The investigation began with evidence acquisition and packet capture validation before identifying the compromised host through endpoint statistics. Network traffic was then analyzed across DNS, HTTP, SMTP, TLS, and TCP protocols to identify external infrastructure, reconstruct the malware's communication patterns, extract indicators of compromise (IOCs), and develop an attack timeline.


## Detection & Analysis Workflow

1. Validated the packet capture and reviewed capture statistics.
2. Identified the compromised workstation using IPv4 Endpoint Statistics.
3. Reviewed the Protocol Hierarchy to identify the primary network protocols.
4. Analyzed DNS requests to identify domains associated with the malware activity.
5. Examined HTTP requests, headers, and transferred objects.
6. Inspected TLS sessions to identify encrypted communications.
7. Reviewed SMTP traffic to identify outbound email communications.
8. Documented indicators of compromise (IOCs).
9. Reconstructed the malware's communication sequence.
10. Mapped observed behaviors to the MITRE ATT&CK framework.
11. Documented the investigation findings.

## Investigation Findings

## Victim Workstation

- **IP Address:** 10.1.9.101
- Identified as the compromised workstation based on IPv4 endpoint statistics and conversation analysis.
- Generated the highest packet count within the capture and initiated DNS, HTTP, TLS, and SMTP communications throughout the investigation.

## Observed Malicious Network Activity

- DNS lookups for external infrastructure.
- HTTP GET requests.
- TLS encrypted communications.
- Multiple outbound connections.
- Public IP lookup activity.


# Indicators of Compromise (IOCs)

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

## MITRE ATT&CK Mapping

The observed network activity was mapped to the MITRE ATT&CK framework to classify adversary behavior using an industry-standard knowledge base. Rather than simply identifying suspicious traffic, MITRE ATT&CK provides standardized techniques that describe how attackers communicate, move data, and accomplish objectives during an intrusion. Mapping the observed evidence to MITRE ATT&CK allows analysts to document incidents consistently, understand attacker behavior, and develop detection and response strategies based on recognized adversary techniques.

### T1071.004 – DNS

The compromised workstation generated multiple DNS queries to resolve external domains before establishing outbound network communications. Analysis of the packet capture identified requests for domains including `api.telegram.org`, `eraqron.shop`, `firebasestorage.googleapis.com`, and other external infrastructure. These DNS lookups demonstrate the malware's reliance on domain name resolution to locate remote systems prior to communication. Mapping this activity to T1071.004 documents the use of DNS as part of the malware's communication process and highlights infrastructure that can be monitored or blocked during future detection efforts.

### T1071.001 – Application Layer Protocol

Following DNS resolution, the infected workstation communicated with external infrastructure using application-layer protocols observed within the packet capture. HTTP requests retrieved remote resources, while SMTP communications were used to transmit information from the compromised system. Because HTTP and SMTP are legitimate protocols commonly permitted through firewalls, attackers frequently abuse them to blend malicious traffic with normal network activity. The observed communications align with MITRE ATT&CK Technique T1071.001 because the malware relied on standard application-layer protocols to conduct its network operations.

### T1041 – Exfiltration Over C2 Channel

Analysis of SMTP traffic over TCP port 587 identified outbound email communications between the infected workstation and the `eraqron.shop` mail infrastructure. Because the SMTP session was unencrypted, the sender and recipient email addresses were visible within the packet capture, providing direct evidence that information was being transmitted from the compromised host. This behavior aligns with MITRE ATT&CK Technique T1041 because the malware used an existing communication channel to transfer collected data to attacker-controlled infrastructure. Identifying this activity demonstrates how packet analysis can reveal evidence of data exfiltration during a network forensic investigation.

---
## Investigation Screenshots

## 1. Evidence Loaded

![Evidence Loaded](Screenshots/01-pcap-loaded.png)

The packet capture was successfully loaded into Wireshark, confirming a total of 5,299 captured packets for analysis. Initial inspection revealed DNS resolution followed by TCP connection establishment and TLS communications originating from the internal workstation (10.1.9.101), providing the starting point for the forensic investigation.
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
## 9. SMTP Data Exfiltration

![SMTP Traffic](Screenshots/SMTP.png)

Analysis of traffic over TCP port 587 identified unencrypted SMTP communications between the infected workstation and **162.254.34.31**. The activity was associated with the `eraqron.shop` infrastructure and was used to transmit victim data through email.

The identified sender and recipient addresses were:

- **Sender:** `rejump@eraqron.shop`
- **Recipient:** `jump@eraqron.shop`

Because the SMTP session was not encrypted, Wireshark could inspect the email protocol activity directly. This provided evidence of post-infection data exfiltration from the compromised workstation.
---
## Lessons Learned

- Correlating DNS, HTTP, TLS, SMTP, and TCP traffic provided significantly more insight than analyzing any single protocol in isolation.
- Encrypted communications remained valuable for investigation because DNS activity, TLS handshake metadata, connection endpoints, and communication patterns continued to reveal attacker infrastructure and malware behavior.
- Documenting indicators of compromise (IOCs) and mapping observed activity to the MITRE ATT&CK framework improved the organization, communication, and operational value of the investigation findings.
- A structured, evidence-based methodology enabled the malware's communication sequence to be reconstructed and supported clear, repeatable forensic findings.

## Conclusion

Analysis of the packet capture identified 10.1.9.101 as the compromised workstation responsible for the majority of the observed network communications associated with the malware infection.Examination of DNS, HTTP, TLS, SMTP, and TCP traffic reconstructed the malware's communication sequence, identified the external infrastructure contacted during execution, and revealed outbound SMTP communications associated with the eraqron.shop mail server. The investigation documented multiple indicators of compromise (IOCs), including observed domains and IP addresses associated with the malware's activity, and mapped the observed adversary behaviors to the MITRE ATT&CK framework using T1071.004 (DNS), T1071.001 (Application Layer Protocol), and T1041 (Exfiltration Over C2 Channel). This investigation demonstrates how systematic packet analysis can identify compromised systems, reconstruct malware communications, and produce evidence-based findings suitable for Digital Forensics and Incident Response (DFIR) investigations.
