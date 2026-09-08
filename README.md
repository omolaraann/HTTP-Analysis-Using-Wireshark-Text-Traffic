# HTTP Traffic Analysis Using Wireshark Text Traffic

**Environment:** Kali Linux
**Protocol:** HTTP over TCP
**Capture:** `basic.pcapng`

---

## 1. Lab Overview

This lab demonstrates the capture and forensic analysis of HTTP traffic using **Wireshark** and command-line network analysis tools.

The objective is to examine a complete HTTP communication session and identify key network events, including:

* TCP three-way handshake
* HTTP GET request
* HTTP 200 OK response
* TCP acknowledgements
* TCP connection termination using FIN/ACK packets
* HTTP request and response details
* TCP stream contents

The analysis was performed against traffic captured on the loopback interface (`127.0.0.1`).

---

## 2. Objectives

The objectives of this lab are to:

1. Capture HTTP traffic generated from a local web request.
2. Analyse the TCP connection establishment process.
3. Identify and examine an HTTP GET request.
4. Analyse the HTTP server response.
5. Identify the TCP connection termination sequence.
6. Use Wireshark to inspect the complete TCP conversation.
7. Preserve and document relevant forensic evidence.
8. Verify captured evidence using command-line tools.

---

## 3. Tools Used

| Tool       | Purpose                                |
| ---------- | -------------------------------------- |
| Wireshark  | Graphical packet and protocol analysis |
| TShark     | Command-line packet analysis           |
| curl       | Generate HTTP requests                 |
| Git/GitHub | Evidence and lab version control       |
| Kali Linux | Analysis environment                   |

---

## 4. Lab Structure

```text
.
├── evidence/
│   └── basic.pcapng
├── exported/
├── reports/
│   ├── capture_hashes.txt
│   ├── case_information.txt
│   ├── connection_close.tsv
│   └── curl_verbose.txt
├── screenshots/
├── scripts/
└── working/
    └── basic_working.pcapng
```

### Directory Description

* **evidence/** — preserved original capture evidence.
* **working/** — working copy used during analysis.
* **reports/** — generated analysis and documentation files.
* **screenshots/** — visual evidence captured during Wireshark analysis.
* **exported/** — exported analysis artefacts.
* **scripts/** — supporting scripts used during the lab.

---

## 5. Network Capture

The analysis focuses on HTTP traffic between the local client and HTTP server:

```text
Client: 127.0.0.1:53484
Server: 127.0.0.1:80
```

The capture contains **20 packets** and includes two HTTP sessions.

The second session, which uses TCP stream 0 in the capture, was examined in detail.

---

## 6. TCP Three-Way Handshake

The second HTTP session begins with the standard TCP three-way handshake:

| Packet | Direction    | TCP Flag | Sequence |
| ------ | ------------ | -------- | -------: |
| 11     | `53484 → 80` | SYN      |        0 |
| 12     | `80 → 53484` | SYN, ACK |        0 |
| 13     | `53484 → 80` | ACK      |        1 |

This establishes the TCP connection before application-layer HTTP communication begins.

---

## 7. HTTP GET Request

Packet **14** contains the HTTP request:

```text
GET /basic.html HTTP/1.1
```

The request is sent from:

```text
127.0.0.1:53484
```

to:

```text
127.0.0.1:80
```

The HTTP request was examined in Wireshark by selecting packet 14 and expanding the **Hypertext Transfer Protocol** section.

Relevant request information includes the requested resource and HTTP request headers.

---

## 8. HTTP 200 OK Response

Packet **16** contains the server response:

```text
HTTP/1.1 200 OK
```

This indicates that the HTTP request was successfully processed and the requested resource was returned by the server.

The response packet was examined in Wireshark to identify the HTTP response headers and returned content.

---

## 9. TCP Connection Termination

The second HTTP session terminates using a normal TCP FIN/ACK exchange.

The relevant packets are:

| Packet | Direction    | Flags    | Sequence | Acknowledgement |
| ------ | ------------ | -------- | -------: | --------------: |
| 18     | `53484 → 80` | FIN, ACK |       84 |             510 |
| 19     | `80 → 53484` | FIN, ACK |      510 |              85 |
| 20     | `53484 → 80` | ACK      |       85 |             511 |

This demonstrates a clean TCP connection termination rather than a TCP reset.

The termination evidence was also exported to:

```text
reports/connection_close.tsv
```

---

## 10. Follow TCP Stream

Wireshark's **Follow TCP Stream** functionality was used to reconstruct the application-layer conversation associated with the HTTP session.

This provides a consolidated view of the communication between the client and server, including the HTTP request and server response.

The corresponding screenshot evidence is stored in:

```text
screenshots/
```

---

## 11. Evidence Files

### Original Evidence

```text
evidence/basic.pcapng
```

This file represents the preserved packet capture used as the primary evidence source.

### Working Evidence

```text
working/basic_working.pcapng
```

A working copy of the capture was used for analysis.

### Generated Reports

```text
reports/capture_hashes.txt
reports/case_information.txt
reports/connection_close.tsv
reports/curl_verbose.txt
```

These files contain supporting information generated during the investigation.

---

## 12. Key Findings

The analysis established the following:

1. A TCP connection was successfully established between the local client and HTTP server.
2. The TCP three-way handshake was observed in packets 11–13.
3. An HTTP GET request for `/basic.html` was observed in packet 14.
4. The server returned an `HTTP/1.1 200 OK` response in packet 16.
5. The HTTP communication occurred over TCP port 80.
6. The connection was terminated normally using FIN/ACK packets 18–20.
7. No TCP reset was identified in the examined connection-termination evidence.
8. The complete HTTP conversation can be reconstructed using Wireshark's Follow TCP Stream functionality.

---

## 13. Conclusion

This lab demonstrated the forensic analysis of HTTP traffic using Wireshark and TShark.

The packet capture provided sufficient evidence to reconstruct the lifecycle of the HTTP session, from TCP connection establishment through HTTP request and response to normal TCP connection termination.

The analysis also demonstrated the importance of preserving the original packet capture while using a separate working copy for investigation and generating supporting evidence files.

---

## 14. Reproducibility

The packet capture can be analysed using Wireshark:

```bash
wireshark evidence/basic.pcapng
```

The TCP connection termination evidence can be reproduced using:

```bash
tshark -r working/basic_working.pcapng \
-Y 'tcp.flags.fin==1 || tcp.flags.reset==1' \
-T fields \
-e frame.number -e frame.time \
-e ip.src -e tcp.srcport \
-e ip.dst -e tcp.dstport \
-e tcp.flags -e tcp.seq -e tcp.ack
```

The output can be saved using:

```bash
tshark -r working/basic_working.pcapng \
-Y 'tcp.flags.fin==1 || tcp.flags.reset==1' \
-T fields \
-e frame.number -e frame.time \
-e ip.src -e tcp.srcport \
-e ip.dst -e tcp.dstport \
-e tcp.flags -e tcp.seq -e tcp.ack \
| tee reports/connection_close.tsv
```

---

## 15. Repository

This repository contains the complete lab artefacts, including the packet capture, working copy, reports, screenshots, exported artefacts, and supporting scripts.

**Author:** Kafayat Animashawun
**Lab:** SBT-DF203 Lab 1
