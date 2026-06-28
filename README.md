Endpoint Visibility and Attack Surface Mapping via Nmap

1. Project Overview
   This project focuses on the foundational mechanics of infrastructure auditing, attack surface minimization, and endpoint visibility. Utilizing Nmap (Network Mapper), this lab conducts structured host discovery, transport-layer port state evaluation, application banner extraction, and heuristic operating system fingerprinting against a secure external target. The goal is to safely isolate open communication vectors and identify potential software version vulnerabilities before they can be leveraged by a threat actor.

3. Technical Architecture & Scan Methodology
The audit progresses through a non-destructive information-gathering pipeline:

[ Target: scanme.nmap.org ] ──> [ ICMP/TCP Host Discovery ] ──> [ TCP SYN Port State Evaluation ]
                                                                             │
                                                                             v
[ NSE Vulnerability Check ] <── [ OS Fingerprint Profiling ] <── [ Application Banner Grab ]

4. Operational Implementation & Execution Logs

Phase 1: Network Host Discovery
Verifies network-layer connectivity and host state without triggering security filtering controls on the target network.

Bash

    nmap -sn scanme.nmap.org

<img width="1256" height="226" alt="image" src="https://github.com/user-attachments/assets/476f4840-9aeb-4f3e-a615-1d5569e5fd70" />

Execution Output:
Plaintext

    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.00059s latency).
    Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
    Nmap done: 1 IP address (1 host up) scanned in 0.16 seconds

Phase 2: Transport Layer Port & Service Version Enumeration
Probes the top 20 high-velocity attack ports to classify socket states and capture application layer banners.

Bash

    nmap -sV --top-ports 20 scanme.nmap.org

<img width="1307" height="797" alt="image" src="https://github.com/user-attachments/assets/6e2168e6-59ff-495d-9944-22899e96652e" />

Execution Output:
Plaintext

    PORT     STATE    SERVICE    VERSION
    22/tcp   open     ssh        OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
    80/tcp   open     http       Apache httpd 2.4.57 ((Debian))
    [... Remaining 18 Top Ports Evaluated to CLOSED State ...]

Phase 3: Heuristic Operating System Fingerprinting
Analyzes TCP window sizes, options, and flags within the network stack response to determine the kernel ecosystem.

Bash
   
    sudo nmap -O --top-ports 20 scanme.nmap.org

<img width="1202" height="805" alt="image" src="https://github.com/user-attachments/assets/944ffe91-4531-48fd-ad3e-60b69518e831" />

Execution Output:
Plaintext

    Device type: general purpose
    Running: Linux 5.X | 6.X
    OS details: Linux 5.0 - 6.6
    Network Distance: 12 hops

Phase 4: Automated Vulnerability Script Auditing
Leverages the Nmap Scripting Engine (NSE) to compare enumerated software versions directly against known CVE public exploit databases.

Bash

    nmap --script=vuln --top-ports 20 scanme.nmap.org

<img width="865" height="627" alt="image" src="https://github.com/user-attachments/assets/bf71f9f2-cb2e-4729-aef4-5c051f345e5a" />

4. Analytical Attack Surface Matrix

Target Asset,Verified Port,Protocol,Service Banner,Exposure Classification
45.33.32.156,22,TCP,OpenSSH 9.2p1 Debian,Authorized Secure Remote Access
45.33.32.156,80,TCP,Apache httpd 2.4.57,Authorized Public Web Server

5. Key Cybersecurity Takeaways

Defensive Asset Visibility: You cannot secure what you do not know exists. Regular network audits establish an objective baseline of exposed ports, preventing unauthorized services ("shadow IT") from expanding the attack surface.

Banner Grabbing and Risk Management: Outdated software versions visible in banner text strings give attackers a clear roadmap for exploitation. Hardening systems by hiding or altering these version banners reduces information disclosure.

Proactive Vulnerability Scanning: Running automated vulnerability scripts helps defensive engineers find and patch missing security updates before malicious actors scan for them.
