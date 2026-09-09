# 🔎 HTTP Traffic Analysis Using Wireshark

**Author:** Omolara Animashawun
**Environment:** Kali Linux
**Primary Protocol:** HTTP over TCP
**Network Interface:** Loopback (`lo`)
**Client:** `127.0.0.1:53484`
**Server:** `127.0.0.1:80`
**Primary Evidence:** `evidence/basic.pcapng`
**Working Evidence:** `working/basic_working.pcapng`

---

## 🔎 Lab Overview

This laboratory exercise demonstrates the **capture, preservation, examination, and forensic analysis of HTTP network traffic** using Wireshark, TShark, and supporting command-line tools.

The purpose of the exercise was to analyse a complete HTTP communication session and reconstruct the sequence of network events that occurred between a local client and HTTP server.

The traffic was captured on the Kali Linux loopback interface (`lo`). As a result, the communication occurred locally between processes on the same host rather than between two external network hosts.

The investigation focused on the complete lifecycle of an HTTP communication session, including:

* TCP connection establishment
* TCP three-way handshake
* HTTP GET request
* HTTP server response
* TCP acknowledgements
* TCP connection termination
* FIN/ACK analysis
* TCP stream reconstruction
* HTTP request and response examination
* Evidence preservation
* Evidence integrity verification
* Independent command-line verification

The lab demonstrates how a packet capture can be examined as digital evidence to reconstruct network activity and establish a chronological sequence of events.

---

## 🎯 Purpose of the Investigation

The purpose of this investigation was to determine whether the captured traffic contained sufficient evidence to reconstruct a complete HTTP transaction.

The investigation specifically sought to establish:

* Whether a TCP connection was successfully established.
* Which endpoints participated in the communication.
* Which TCP ports were used.
* Whether an HTTP GET request was transmitted.
* Which resource was requested.
* Whether the server successfully processed the request.
* Which HTTP status code was returned.
* Whether the complete HTTP conversation could be reconstructed.
* How the TCP connection was terminated.
* Whether the connection ended normally or through a TCP reset.
* Whether the findings could be independently verified using TShark.
* Whether the original packet capture was preserved separately from the working copy.

---

## 🎯 Objectives

The objectives of this laboratory exercise were to:

* Capture HTTP traffic generated from a local web request.
* Analyse the TCP connection establishment process.
* Identify and examine an HTTP GET request.
* Analyse the corresponding HTTP server response.
* Identify the TCP connection termination sequence.
* Reconstruct the HTTP conversation using Wireshark.
* Preserve and document relevant forensic evidence.
* Verify captured evidence using command-line tools.
* Document evidence integrity using cryptographic hashing.
* Produce a reproducible record of the investigation.

---

## 💻 Analysis Environment

The investigation was conducted in a Kali Linux environment.

The following tools were used:

| Tool           | Purpose                                              |
| -------------- | ---------------------------------------------------- |
| **Wireshark**  | Graphical packet and protocol analysis               |
| **TShark**     | Command-line packet analysis and evidence extraction |
| **curl**       | Generation and verification of HTTP requests         |
| **Git**        | Version control and repository management            |
| **GitHub**     | Repository hosting and version history               |
| **Kali Linux** | Analysis and testing environment                     |

---

## 🌐 Network Architecture

The HTTP communication analysed in this laboratory occurred entirely on the local host.

```text
                 Loopback Interface
                      127.0.0.1
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
    ┌─────────────────┐        ┌─────────────────┐
    │   HTTP Client   │        │   HTTP Server   │
    │                 │        │                 │
    │ 127.0.0.1:53484 │◄──────►│  127.0.0.1:80  │
    │                 │  HTTP  │                 │
    └─────────────────┘        └─────────────────┘
```

### Client

```text
127.0.0.1:53484
```

The client uses an ephemeral TCP source port, `53484`.

### Server

```text
127.0.0.1:80
```

The server listens on the standard HTTP TCP port, `80`.

### Interface

```text
lo
```

The `lo` interface is the Linux loopback interface used for local host-to-host communication.

---

## 📁 Repository Structure

The laboratory repository is organised as follows:

```text
.
├── evidence/
│   └── basic.pcapng
│
├── exported/
│
├── reports/
│   ├── capture_hashes.txt
│   ├── case_information.txt
│   ├── connection_close.tsv
│   └── curl_verbose.txt
│
├── screenshots/
│
├── scripts/
│
└── working/
    └── basic_working.pcapng
```

### 📂 `evidence/`

Contains the preserved original packet capture.

```text
evidence/basic.pcapng
```

This is the primary network evidence used in the investigation.

### 📂 `working/`

Contains the working copy of the packet capture.

```text
working/basic_working.pcapng
```

The working copy was used during packet inspection and evidence extraction.

### 📂 `reports/`

Contains supporting analysis and documentation:

```text
reports/
├── capture_hashes.txt
├── case_information.txt
├── connection_close.tsv
└── curl_verbose.txt
```

### 📂 `screenshots/`

Contains screenshots captured during Wireshark analysis.

### 📂 `exported/`

Contains exported analysis artefacts.

### 📂 `scripts/`

Contains supporting scripts used during the laboratory exercise, where applicable.

---

## 🔐 Evidence Preservation

The original packet capture was preserved in:

```text
evidence/basic.pcapng
```

A separate working copy was created:

```text
working/basic_working.pcapng
```

The analysis workflow therefore followed this basic structure:

```text
Original Capture
      │
      ▼
evidence/basic.pcapng
      │
      │  Working Copy
      ▼
working/basic_working.pcapng
      │
      ├───────────────┐
      ▼               ▼
 Wireshark         TShark
      │               │
      └───────┬───────┘
              ▼
     Analysis Evidence
              │
       ┌──────┴──────┐
       ▼             ▼
    Reports      Screenshots
```

Maintaining a separate working copy allows analysis to be performed while retaining the original capture as the preserved source evidence.

---

## 🔒 Evidence Integrity

Evidence integrity was documented using cryptographic hashing.

Hash information is stored in:

```text
reports/capture_hashes.txt
```

The purpose of hashing is to provide a reproducible value that can be used to verify whether the corresponding evidence file has changed.

The original packet capture remains stored separately from the working analysis copy.

This provides an additional layer of evidence-handling discipline for the investigation.

---

## 📡 Capture Overview

The packet capture contains:

```text
20 packets
```

The capture contains two HTTP sessions.

The **second HTTP session** was selected for detailed analysis because it provides the complete TCP and HTTP communication sequence required for this laboratory exercise.

The selected session involves:

```text
Client:  127.0.0.1:53484
Server:  127.0.0.1:80
```

The relevant protocols are:

```text
Application:  HTTP
Transport:    TCP
Network:      IPv4
Interface:    Loopback (lo)
```

---

## 🤝 TCP Three-Way Handshake

TCP uses a three-way handshake to establish a connection before application data is exchanged.

The selected HTTP session begins with the following packets:

| Packet | Source            | Destination       | TCP Flag | Sequence |
| -----: | ----------------- | ----------------- | -------- | -------: |
|     11 | `127.0.0.1:53484` | `127.0.0.1:80`    | SYN      |        0 |
|     12 | `127.0.0.1:80`    | `127.0.0.1:53484` | SYN, ACK |        0 |
|     13 | `127.0.0.1:53484` | `127.0.0.1:80`    | ACK      |        1 |

### 📤 Packet 11 — SYN

Packet 11 contains the initial SYN packet sent by the client.

```text
127.0.0.1:53484
        │
        │ SYN
        ▼
127.0.0.1:80
```

The SYN flag indicates that the client is attempting to establish a TCP connection with the HTTP server.

### 📥 Packet 12 — SYN/ACK

Packet 12 is sent by the server.

```text
127.0.0.1:80
        │
        │ SYN, ACK
        ▼
127.0.0.1:53484
```

The SYN/ACK acknowledges the client's connection request and synchronises the server's TCP sequence numbering.

### ✅ Packet 13 — ACK

Packet 13 is transmitted by the client.

The ACK completes the TCP three-way handshake.

At this point, the TCP connection has been established and application-layer HTTP communication can begin.

---

## 📤 HTTP GET Request

After the TCP connection was established, the client transmitted an HTTP request.

The relevant packet is:

```text
Packet 14
```

The HTTP request contains:

```http
GET /basic.html HTTP/1.1
```

The request was sent from:

```text
127.0.0.1:53484
```

to:

```text
127.0.0.1:80
```

The request was examined in Wireshark by selecting packet 14 and expanding the **Hypertext Transfer Protocol** section.

---

## 🔎 HTTP Request Analysis

The important request information identified from packet 14 is:

| Field              | Value             |
| ------------------ | ----------------- |
| HTTP Method        | `GET`             |
| Requested Resource | `/basic.html`     |
| HTTP Version       | `HTTP/1.1`        |
| Source             | `127.0.0.1:53484` |
| Destination        | `127.0.0.1:80`    |

The HTTP GET method indicates that the client requested a resource from the server.

The requested resource was:

```text
/basic.html
```

This confirms that the TCP connection established in packets 11–13 was subsequently used to transport application-layer HTTP traffic.

---

## 📥 HTTP 200 OK Response

The server response was identified in:

```text
Packet 16
```

The HTTP response contains:

```http
HTTP/1.1 200 OK
```

The response was transmitted from:

```text
127.0.0.1:80
```

to:

```text
127.0.0.1:53484
```

---

## ✅ Interpretation of HTTP 200 OK

The `200 OK` HTTP status indicates that the server successfully processed the client's request.

The response provides evidence that:

1. The TCP connection was established.
2. The HTTP request reached the server.
3. The server processed the request.
4. The requested resource was successfully returned.

The response packet was examined in Wireshark to identify the HTTP response headers and returned content.

---

## ↔️ TCP Acknowledgements

TCP acknowledgements were observed throughout the communication.

Acknowledgement values allow the analyst to establish how TCP data was exchanged between the client and server.

The sequence and acknowledgement numbers also assist in establishing the order of events within the TCP conversation.

This information is particularly useful during forensic reconstruction because packet captures must often be interpreted as a sequence of related events rather than as isolated packets.

---

## 🔚 TCP Connection Termination

After the HTTP communication was completed, the TCP connection was terminated using a normal FIN/ACK exchange.

The relevant packets are:

| Packet | Source            | Destination       | Flags    | Sequence | Acknowledgement |
| -----: | ----------------- | ----------------- | -------- | -------: | --------------: |
|     18 | `127.0.0.1:53484` | `127.0.0.1:80`    | FIN, ACK |       84 |             510 |
|     19 | `127.0.0.1:80`    | `127.0.0.1:53484` | FIN, ACK |      510 |              85 |
|     20 | `127.0.0.1:53484` | `127.0.0.1:80`    | ACK      |       85 |             511 |

---

## 🧩 Interpretation of Connection Termination

Packet 18 contains the FIN flag transmitted by the client.

The server responds with packet 19, which contains both FIN and ACK flags.

The client then sends packet 20 containing the final ACK.

The sequence demonstrates a normal TCP connection teardown.

The examined termination sequence does **not** contain a TCP RST packet.

Therefore, the selected connection appears to have ended normally rather than being forcibly reset.

---

## 🧪 TShark FIN/RESET Analysis

The TCP termination evidence was independently extracted using TShark.

The following filter was used:

```text
tcp.flags.fin==1 || tcp.flags.reset==1
```

This identifies packets containing either:

* TCP FIN flags, or
* TCP RESET flags.

The extracted evidence was saved to:

```text
reports/connection_close.tsv
```

This provides a command-line record of the packets associated with connection termination.

---

## 🔄 Follow TCP Stream

Wireshark's **Follow TCP Stream** functionality was used to reconstruct the application-layer communication.

This functionality associates packets belonging to the same TCP conversation and presents the communication as a consolidated stream.

The reconstructed conversation provides a clearer view of the communication between:

```text
Client:
127.0.0.1:53484
```

and:

```text
Server:
127.0.0.1:80
```

The reconstructed stream allows the analyst to observe the relationship between the HTTP request and the corresponding server response.

Visual evidence from this analysis is stored in:

```text
screenshots/
```

---

## ⏱️ Packet Timeline

The selected HTTP session can be represented chronologically as follows:

```text
┌───────────────────────────────┐
│ Packet 11                     │
│ Client → Server               │
│ TCP SYN                       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Packet 12                     │
│ Server → Client               │
│ TCP SYN/ACK                   │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Packet 13                     │
│ Client → Server               │
│ TCP ACK                       │
└───────────────┬───────────────┘
                │
                ▼
       TCP CONNECTION
         ESTABLISHED
                │
                ▼
┌───────────────────────────────┐
│ Packet 14                     │
│ Client → Server               │
│ GET /basic.html HTTP/1.1      │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Packet 16                     │
│ Server → Client               │
│ HTTP/1.1 200 OK               │
└───────────────┬───────────────┘
                │
                ▼
        TCP ACKNOWLEDGEMENTS
                │
                ▼
┌───────────────────────────────┐
│ Packet 18                     │
│ Client → Server               │
│ FIN/ACK                       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Packet 19                     │
│ Server → Client               │
│ FIN/ACK                       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Packet 20                     │
│ Client → Server               │
│ ACK                           │
└───────────────┬───────────────┘
                │
                ▼
        TCP CONNECTION CLOSED
```

This timeline demonstrates the progression from connection establishment to application-layer communication and finally connection termination.

---

## 🕵️ Forensic Significance

The packet capture provides sufficient evidence to reconstruct the selected HTTP transaction.

The investigation established the following sequence:

### Connection Initiation

The client initiated a TCP connection to the HTTP server using a SYN packet.

### Connection Establishment

The server responded with SYN/ACK and the client completed the handshake using ACK.

### HTTP Request

The client requested:

```text
/basic.html
```

using:

```text
GET /basic.html HTTP/1.1
```

### HTTP Response

The server returned:

```text
HTTP/1.1 200 OK
```

indicating successful processing of the request.

### Connection Termination

The TCP session subsequently ended through a FIN/ACK exchange.

The combined evidence provides a coherent chronological reconstruction of the HTTP transaction.

---

## 📸 Visual Evidence

Screenshots captured during the Wireshark investigation are stored in:

```text
screenshots/
```

The screenshots provide visual documentation of relevant observations, including:

* TCP three-way handshake.
* HTTP GET request.
* HTTP response.
* Packet details.
* HTTP protocol fields.
* Follow TCP Stream output.
* TCP FIN/ACK termination.

The screenshots complement the packet-level and command-line evidence contained in the repository.

---

## 🧾 Supporting Evidence

The investigation includes several supporting artefacts.

### Primary Evidence

```text
evidence/basic.pcapng
```

Original preserved packet capture.

### Working Evidence

```text
working/basic_working.pcapng
```

Working copy used for analysis.

### Evidence Hashes

```text
reports/capture_hashes.txt
```

Contains cryptographic hash information used to document evidence integrity.

### Case Information

```text
reports/case_information.txt
```

Contains case and laboratory identification information.

### TCP Termination Evidence

```text
reports/connection_close.tsv
```

Contains TShark output identifying FIN and RESET packets.

### HTTP Verification

```text
reports/curl_verbose.txt
```

Contains verbose HTTP request information generated using `curl`.

---

## 🧪 HTTP Verification Using curl

The laboratory also used `curl` to generate and/or verify the HTTP request.

The verbose output was retained in:

```text
reports/curl_verbose.txt
```

The verbose output provides supporting information about the HTTP communication, including connection and request/response behaviour.

The packet capture remains the primary source of network evidence, while the curl output serves as supporting evidence for the generated HTTP transaction.

---

## 🖥️ Wireshark Analysis Workflow

The following workflow was used during the packet analysis:

### Open the Capture

```bash
wireshark evidence/basic.pcapng
```

### Identify HTTP/TCP Traffic

The packets were examined to identify the HTTP sessions and associated TCP conversations.

### Select the Target Session

The second HTTP session was selected for detailed examination.

### Examine the TCP Handshake

Packets 11–13 were examined to confirm successful TCP connection establishment.

### Examine the HTTP Request

Packet 14 was inspected to identify:

```text
GET /basic.html HTTP/1.1
```

### Examine the HTTP Response

Packet 16 was inspected to confirm:

```text
HTTP/1.1 200 OK
```

### Reconstruct the TCP Stream

Wireshark's **Follow TCP Stream** functionality was used to reconstruct the application-layer conversation.

### Examine Connection Termination

Packets 18–20 were examined to identify the FIN/ACK termination sequence.

### Export Supporting Evidence

Relevant TCP termination evidence was extracted using TShark.

### Preserve Evidence

The original capture remained separate from the working analysis copy.

---

## 🎛️ Recommended Wireshark Filters

The following display filters can be used when examining the capture.

### HTTP Traffic

```text
http
```

### TCP Traffic

```text
tcp
```

### TCP Port 80

```text
tcp.port == 80
```

### Client Port

```text
tcp.port == 53484
```

### FIN Packets

```text
tcp.flags.fin == 1
```

### TCP Reset Packets

```text
tcp.flags.reset == 1
```

### FIN or RESET Packets

```text
tcp.flags.fin == 1 || tcp.flags.reset == 1
```

### Selected TCP Stream

```text
tcp.stream == 0
```

---

## 🔬 TShark Verification

The TCP connection termination evidence can be reproduced from the working capture using:

```bash
tshark -r working/basic_working.pcapng \
-Y 'tcp.flags.fin==1 || tcp.flags.reset==1' \
-T fields \
-e frame.number \
-e frame.time \
-e ip.src \
-e tcp.srcport \
-e ip.dst \
-e tcp.dstport \
-e tcp.flags \
-e tcp.seq \
-e tcp.ack
```

The command extracts:

* Frame number
* Timestamp
* Source IP address
* Source TCP port
* Destination IP address
* Destination TCP port
* TCP flags
* TCP sequence number
* TCP acknowledgement number

The output can be saved to the report using:

```bash
tshark -r working/basic_working.pcapng \
-Y 'tcp.flags.fin==1 || tcp.flags.reset==1' \
-T fields \
-e frame.number \
-e frame.time \
-e ip.src \
-e tcp.srcport \
-e ip.dst \
-e tcp.dstport \
-e tcp.flags \
-e tcp.seq \
-e tcp.ack \
| tee reports/connection_close.tsv
```

---

## 📊 Evidence Summary

| Evidence Category                 | Finding                |
| --------------------------------- | ---------------------- |
| Capture                           | `basic.pcapng`         |
| Total packets                     | 20                     |
| Network interface                 | Loopback (`lo`)        |
| Client                            | `127.0.0.1:53484`      |
| Server                            | `127.0.0.1:80`         |
| Transport protocol                | TCP                    |
| Application protocol              | HTTP                   |
| TCP handshake                     | Packets 11–13          |
| HTTP request                      | Packet 14              |
| HTTP method                       | GET                    |
| Requested resource                | `/basic.html`          |
| HTTP version                      | HTTP/1.1               |
| HTTP response                     | Packet 16              |
| HTTP status                       | `200 OK`               |
| TCP termination                   | Packets 18–20          |
| Termination method                | FIN/ACK                |
| TCP reset in examined termination | Not observed           |
| TCP stream reconstruction         | Successfully performed |
| Evidence hash                     | Documented             |
| Original evidence                 | Preserved              |
| Working copy                      | Created                |
| TShark verification               | Completed              |
| Supporting curl output            | Preserved              |

---

## 🔍 Key Findings

The investigation established that:

* The packet capture contains **20 packets**.
* Two HTTP sessions are present in the capture.
* The second session was selected for detailed analysis.
* The client endpoint was `127.0.0.1:53484`.
* The server endpoint was `127.0.0.1:80`.
* The communication used HTTP over TCP.
* TCP connection establishment occurred in packets **11–13**.
* Packet **11** contains the client SYN.
* Packet **12** contains the server SYN/ACK.
* Packet **13** contains the client ACK completing the handshake.
* Packet **14** contains an HTTP GET request.
* The requested resource was **`/basic.html`**.
* The HTTP request used **HTTP/1.1**.
* Packet **16** contains an **HTTP/1.1 200 OK** response.
* The server successfully processed the request.
* The application-layer conversation could be reconstructed using **Follow TCP Stream**.
* Packets **18–20** contain the TCP termination sequence.
* The connection terminated through a normal **FIN/ACK** exchange.
* No TCP reset was identified in the examined termination sequence.
* TShark was used to independently extract the termination evidence.
* Evidence integrity information was documented using cryptographic hashing.
* The original packet capture was retained separately from the working copy.
* Supporting reports and screenshots were generated as part of the investigation.

---

## 🛡️ Digital Forensics Relevance

Although this laboratory focuses on a simple HTTP transaction, the workflow demonstrates several fundamental principles used in digital-forensics investigations.

### Evidence Identification

Relevant packets were identified using:

* Packet numbers
* IP addresses
* TCP ports
* TCP flags
* Sequence numbers
* HTTP methods
* HTTP status codes
* TCP stream information

### Evidence Preservation

The original packet capture was preserved separately from the working copy.

### Evidence Integrity

Cryptographic hashing was used to document the integrity of the captured evidence.

### Evidence Examination

Wireshark was used to examine individual packets and protocol-layer information.

### Evidence Reconstruction

Follow TCP Stream was used to reconstruct the application-layer communication.

### Independent Verification

TShark was used to independently extract TCP termination evidence.

### Documentation

The findings were documented through:

* Reports
* Command-line output
* Packet analysis
* Screenshots
* Repository artefacts

This workflow can be applied to more complex network-forensics investigations where analysts need to reconstruct communication from packet-level evidence.

---

## ⚠️ Limitations

The analysis has several limitations.

### Loopback Traffic

The traffic was captured on the local loopback interface.

Therefore, the communication represents traffic between processes on the same host rather than communication between separate physical or external hosts.

### HTTP Rather Than HTTPS

The exercise uses HTTP over TCP port 80.

Because the traffic is not encrypted, the HTTP request and response can be inspected directly within the packet capture.

### Controlled Laboratory Environment

The communication was generated specifically for the laboratory exercise.

The findings should therefore be interpreted as controlled laboratory evidence rather than production network activity.

### Scope of Analysis

The investigation focuses on the selected HTTP session.

It does not constitute a complete security assessment of the Kali Linux system or the HTTP server.

---

## 🔁 Reproducibility

The original packet capture can be opened using:

```bash
wireshark evidence/basic.pcapng
```

The working copy can be opened using:

```bash
wireshark working/basic_working.pcapng
```

The TCP termination evidence can be reproduced using:

```bash
tshark -r working/basic_working.pcapng \
-Y 'tcp.flags.fin==1 || tcp.flags.reset==1' \
-T fields \
-e frame.number \
-e frame.time \
-e ip.src \
-e tcp.srcport \
-e ip.dst \
-e tcp.dstport \
-e tcp.flags \
-e tcp.seq \
-e tcp.ack
```

The results can be saved using:

```bash
tshark -r working/basic_working.pcapng \
-Y 'tcp.flags.fin==1 || tcp.flags.reset==1' \
-T fields \
-e frame.number \
-e frame.time \
-e ip.src \
-e tcp.srcport \
-e ip.dst \
-e tcp.dstport \
-e tcp.flags \
-e tcp.seq \
-e tcp.ack \
| tee reports/connection_close.tsv
```

The commands above allow another analyst to reproduce the TCP termination analysis from the working packet capture.

---

## 📦 Repository Contents

The completed repository contains the following artefacts:

```text
evidence/
└── basic.pcapng
```

**Original preserved packet capture.**

```text
working/
└── basic_working.pcapng
```

**Working copy used during analysis.**

```text
reports/
├── capture_hashes.txt
├── case_information.txt
├── connection_close.tsv
└── curl_verbose.txt
```

**Supporting forensic reports and command-line evidence.**

```text
screenshots/
```

**Visual evidence from Wireshark analysis.**

```text
exported/
```

**Exported analysis artefacts.**

```text
scripts/
```

**Supporting scripts used during the laboratory exercise.**

---

## 📝 Conclusion

This laboratory exercise successfully demonstrated the capture, preservation, examination, and forensic analysis of HTTP traffic using Wireshark and TShark.

The selected TCP session was reconstructed from connection establishment through application-layer communication and finally connection termination.

The investigation identified the TCP three-way handshake in packets **11–13**, followed by an HTTP GET request for **`/basic.html`** in packet **14**. The server subsequently returned an **`HTTP/1.1 200 OK`** response in packet **16**, confirming successful processing of the request.

The session was then terminated normally using FIN/ACK packets **18–20**. No TCP reset was identified in the examined termination sequence.

Wireshark's **Follow TCP Stream** functionality provided a consolidated representation of the HTTP conversation, while TShark was used to independently extract and verify TCP connection termination evidence.

From an evidence-handling perspective, the original packet capture was retained in the `evidence/` directory while a separate working copy was used for analysis. Cryptographic hash information was also documented to support evidence integrity.

Overall, the exercise demonstrates how packet captures can be used to reconstruct network activity, identify protocol-level events, preserve digital evidence, and produce a reproducible forensic analysis.

---

## 👤 Author

**Omolara Animashawun**

**Topic:** HTTP Traffic Analysis Using Wireshark
**Platform:** Kali Linux
