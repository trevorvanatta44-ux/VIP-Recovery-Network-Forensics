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


## Tools and Resources

- Wireshark
- MITRE ATT&CK Framework
- Malware-Traffic-Analysis.net (VIP recovery PCAP)

## Skills Demonstrated

- Network Forensics
- Malware Traffic Analysis
- TCP/IP Analysis
- DNS Analysis
- HTTP Analysis
- TLS Traffic Analysis
- Indicator of Compromise (IOC) Identification
- Communication Sequence Reconstruction
- MITRE ATT&CK Mapping

## Investigation Workflow

The investigation began by reviewing the packet capture before identifying the compromised workstation using endpoint statistics. Network traffic was then examined across DNS, HTTP, SMTP, TLS, and TCP protocols to analyze the observed network activity, identify external systems contacted by the compromised workstation, reconstruct the observed network communication sequence, and extract indicators of compromise (IOCs). The resulting findings were documented and mapped to the MITRE ATT&CK framework.

## Detection & Analysis Workflow

1. Validated the packet capture and reviewed capture statistics.
2. Identified the compromised workstation using IPv4 Endpoint Statistics.
3. Reviewed the Protocol Hierarchy to identify the primary network protocols.
4. Analyzed DNS requests to identify observed domain lookups.
5. Examined HTTP requests, headers, and transferred objects.
6. Inspected TLS sessions to identify encrypted communications.
7. Reviewed SMTP traffic to identify outbound email communications.
8. Documented indicators of compromise (IOCs).
9. Reconstructed the observed network communication sequence.
10. Mapped observed network behaviors to the MITRE ATT&CK framework.

## Investigation Findings

## Victim Workstation

- **IP Address:** `10.1.9.101`
- Identified as the compromised workstation based on IPv4 Endpoint Statistics and IPv4 Conversation analysis.
- Generated the highest volume of observed network activity within the packet capture.
- Initiated DNS, HTTP, TLS, and SMTP communications with multiple external systems throughout the investigation.
- Served as the primary focus of the network forensic analysis because it was responsible for the majority of the observed communications.

## Observed Network Communication Sequence

The observed communication sequence was reconstructed by correlating evidence across multiple network protocols observed throughout the packet capture.

1. **DNS Resolution:** The compromised workstation first generated DNS queries to resolve external domain names, including `api.telegram.org`, `eraqron.shop`, and other observed domains. DNS resolution was required before the workstation could establish communications with systems identified by domain name.

2. **TCP Connection Establishment:** After receiving DNS responses, the workstation established TCP connections with the resolved external IP addresses. TCP establishes a reliable connection through a three-way handshake (SYN, SYN/ACK, ACK) before application-layer protocols such as HTTP, TLS, or SMTP exchange data.

3. **TLS Communications:** After TCP connections were established, TLS handshakes were observed between the workstation and several external systems. The TLS handshake negotiated encrypted sessions before application data was exchanged. While the encrypted payloads could not be inspected, the handshake metadata and connection information remained visible for analysis.
   
4. **HTTP Communications:** HTTP requests and responses were observed over the established TCP connections, allowing the workstation to communicate with external web resources and retrieve remote content.

5. **SMTP Communications:** Later in the capture, the workstation established an SMTP session over TCP port 587 with the `eraqron.shop` mail server. The session successfully authenticated before transmitting an outbound email from `rejump@eraqron.shop` to `jump@eraqron.shop`.

## Indicators of Compromise (IOCs)

The following domains and IP addresses were observed during the investigation and are documented as indicators associated with the captured network activity. Their inclusion does not necessarily indicate that every domain or IP address is inherently malicious; some represent legitimate services contacted during the observed communications.

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

## MITRE ATT&CK Mapping

The observed network behaviors were mapped to the MITRE ATT&CK framework to classify adversary techniques identified during the investigation.

| Technique ID | Technique | Evidence Observed |
|--------------|-----------|-------------------|
| T1071.004 | DNS | DNS queries were observed resolving external domains including `api.telegram.org`, `eraqron.shop`, `firebasestorage.googleapis.com`, and other domains before outbound communications were established. |
| T1071.001 | Application Layer Protocol | HTTP and SMTP application-layer protocols were observed communicating with external systems throughout the investigation. |
| T1041 | Exfiltration Over C2 Channel | An outbound SMTP session over TCP port 587 was observed between the compromised workstation and the `eraqron.shop` mail server. Sender and recipient email addresses were visible because the SMTP session was unencrypted. |

---
# Investigation Screenshots
---
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

Reviewing transferred objects helped identify files and additional artifacts associated with the investigation.
---
## 9. SMTP Analysis

![SMTP Traffic](Screenshots/SMTP.png)

Analysis of traffic over TCP port 587 identified unencrypted SMTP communications between the compromised workstation and 162.254.34.31. The SMTP session involved the domain `eraqron.shop` and included the following email addresses:

- **Sender:** `rejump@eraqron.shop`
- **Recipient:** `jump@eraqron.shop`

Because the SMTP session was unencrypted, the sender and recipient email addresses were visible within the packet capture.

---
## Lessons Learned

- Correlating DNS, HTTP, TLS, SMTP, and TCP traffic provided significantly more insight than analyzing any single protocol in isolation.
- Encrypted communications remained valuable for investigation because DNS activity, TLS handshake metadata, connection endpoints, and communication patterns continued to reveal network activity and communication behavior.
- Documenting indicators of compromise (IOCs) and mapping observed activity to the MITRE ATT&CK framework improved the organization, communication, and operational value of the investigation findings.
- A structured, evidence-based methodology enabled the observed network communication sequence to be reconstructed and supported clear, repeatable forensic findings.

## Conclusion

Analysis of the packet capture identified 10.1.9.101 as the compromised workstation responsible for the majority of the observed network communications. Examination of DNS, HTTP, TLS, SMTP, and TCP traffic reconstructed the observed network communication sequence, identified the external systems contacted by the compromised workstation, and documented outbound SMTP communications involving the eraqron.shop mail server. The investigation documented multiple indicators of compromise (IOCs), including observed domains and IP addresses associated with the malware's activity, and mapped the observed adversary behaviors to the MITRE ATT&CK framework using T1071.004 (DNS), T1071.001 (Application Layer Protocol), and T1041 (Exfiltration Over C2 Channel). This investigation demonstrates how systematic packet analysis can identify compromised systems, reconstruct observed network communications, and produce evidence-based findings.
