# Protocol Evidence Matrix

| Protocol | Display / Selection Filter | Representative Frames | Endpoints | Observed Operation | Security Significance |
|---|---|---:|---|---|---|
| HTTP | `http.authbasic` / frame 15 | 15 | 192.168.2.111 → 192.168.88.115 | HTTP GET `/view/` using Basic Authentication | Basic Authentication uses reversible Base64 encoding. If HTTP is not protected by TLS, captured authentication material may be recovered by an observer. |
| FTP | `ftp` / frame 12002 | 12002 | 192.168.2.206 → 10.10.10.50 | FTP server banner identifying an AXIS 206 Network Camera 4.40 | Cleartext FTP can expose authentication exchanges and service/device metadata. Credential values were intentionally excluded from submitted evidence. |
| Modbus/TCP | `frame.number >= 15001 && frame.number <= 15004` | 15001–15004 | 192.168.2.111 → 10.10.20.12 | FC43 Read Device Identification; FC01 Read Coils; FC03 Read Holding Registers; FC06 Write Single Register | Passive analysis reveals industrial operations, device relationships and read/write activity. |
| DNP3-shaped TCP traffic | `tcp.port == 20000 && tcp.len > 0` / frame 115001 | 115001 | 10.10.10.50 → 10.10.30.20 | Payload begins `05 64`, but the instructor-generated frame contains invalid zero CRC fields and is not decoded as DNP3 by Wireshark | Demonstrates the importance of validating protocol structure rather than identifying a protocol solely by TCP port. |
| EtherNet/IP | `enip` / frame 143001 | 143001 | 10.10.10.50 → 10.10.20.11 | Register Session Request | EtherNet/IP traffic can expose industrial session and endpoint relationships. |
| S7comm | `s7comm` / frames 155001 onward | 155001–155005 | 10.10.10.50 → 10.10.20.11 | S7 Job — Read Var | Passive observation can reveal PLC communication relationships and variable-access patterns. |
