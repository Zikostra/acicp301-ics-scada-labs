# Lab 07 — Protocol Evidence Matrix

| Protocol | Display Filter | Source → Destination | Observed Operation / Evidence | Security Significance |
|---|---|---|---|---|
| HTTP | `http.authbasic` / `http.request.method == "POST"` | 192.168.2.42 → 192.168.88.25 and other HTTP endpoints | HTTP Basic Authentication was present in 21 packets. POST requests were also observed, including `/goform/svLogin` and `/home.asp`. | Basic Authentication material is observable in packet captures because Base64 encoding does not provide confidentiality. Cleartext HTTP can also expose request paths and submitted data. |
| FTP | `ftp` | 192.168.2.64 → 192.168.88.49 / 192.168.88.25 | FTP USER and PASS commands were present. Device/service banners included an AXIS 206 Network Camera and Sockets FTP v7.10. | FTP provides no native encryption, allowing authentication commands and service metadata to be observed by a passive analyst. |
| Modbus/TCP | `modbus` | 192.168.2.42 ↔ 192.168.88.100 | Function 43/14-style device-identification activity was decoded as Read Device Identification request/response traffic. | Modbus/TCP commonly lacks native confidentiality and authentication. Device-identification responses may reveal useful asset metadata. |
| DNP3 | `dnp3` | 10.0.0.9 ↔ 10.0.0.3 | Supplemental capture frames 82–83 show Read Binary Input Change followed by a response. | Passive DNP3 inspection can reveal control-system roles, polling activity, data types and operational relationships when communications are not cryptographically protected. |
| EtherNet/IP | `enip` | 10.1.1.167 ↔ 10.1.1.164 | Supplemental capture frames 6–7 show List Identity request/response; response identifies a 1756-ENBT/A device. | Identity responses expose industrial device type and product information that can assist asset discovery and network reconnaissance. |
| S7comm | `s7comm` | 10.10.10.20 ↔ 10.10.10.10 | Frames 2 and 4 show an S7 Read Var Job followed by Ack_Data. | Repeated S7 variable-read traffic exposes PLC communication relationships and process-access patterns to a passive observer. |

## Dataset qualification

The primary dataset was `4SICS-GeekLounge-151021.pcap`.

DNP3 and EtherNet/IP were not confirmed as genuine application traffic in that
primary capture. Packets using DNP3/EtherNet-IP-associated ports were examined,
but the primary capture did not decode them as those protocols.

Public ICS training captures were therefore used as supplemental study material
for the DNP3 and EtherNet/IP rows. They must remain identified as supplemental
rather than represented as traffic from the primary 4SICS capture.
