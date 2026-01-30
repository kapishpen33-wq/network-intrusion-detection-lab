# Endpoint Security Verification: TCP SYN Scan Analysis

## Executive Summary
In this project, I performed a vulnerability assessment on a local endpoint to verify its network hardening status. Using Nmap for reconnaissance and Wireshark for deep-packet inspection, I confirmed that critical service ports (HTTP, HTTPS, RDP) were correctly closed to unauthorized access.

## Objective
To simulate an external reconnaissance attempt and analyze the TCP/IP stack's response to unauthorized connection attempts (TCP SYN Scans), validating the system's "Default Deny" posture.

## Tools Used
- **Nmap:** Configured for TCP SYN (Stealth) Scanning (`-sS`).
- **Wireshark:** Used for capturing Loopback traffic and analyzing TCP flags.

## Execution & Analysis

### 1. The Vulnerability Scan
I executed a targeted Nmap scan against the localhost to identify potential entry points on ports 80 (Web), 443 (Secure Web), and 3389 (Remote Desktop).

**Command Executed:**
`sudo nmap -sS -p 80,443,3389 127.0.0.1`

![Nmap Result]([Network Intrusion Lab Nmap Commands.png])
*Ref 1: Nmap confirming all target ports are in a "closed" state.*

### 2. Deep Packet Inspection (The "Why")
While Nmap provided a summary, I used Wireshark to verify the findings at the packet level.

![Packet Analysis]([Network Intrusion Lab Wireshark Capture.png])
*Ref 2: Wireshark capture showing the TCP handshake rejection.*

**Technical Observation:**
* **Frame 1 (The Attack):** The source sent a TCP packet with the **SYN** flag set to Port 443.
* **Frame 2 (The Defense):** The target machine immediately responded with **RST, ACK** (Reset, Acknowledge).

**Conclusion:**
The presence of the **RST** flag confirms that the operating system's TCP stack actively rejected the connection attempt. This verifies that no vulnerable services are listening on these ports, validating the endpoint's security hardening.
