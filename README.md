# ele-task-1
Nmap scannig

 Task 1: Local Network Port Scanning and Service Exposure Analysis

## Task Objective

The objective of this task was to perform **basic network reconnaissance** on a local network using **Nmap** to discover active hosts and identify **open ports** (network services). This exercise demonstrates fundamental skills in understanding network service exposure and identifying potential security risks.

## Tools Used

  * **Nmap (Network Mapper):** Used to perform the TCP SYN scan and discover open ports.
  * **Command prompt **
##  Scan Details

The scan was executed against the local IP range using the **TCP SYN Scan** (`-sS`) method, which is fast and stealthy.

### Command Executed

The command used was similar to:

```bash
nmap -sS 192.XXX.XXX.0/24
```

*(Note: IP range has been masked for security.)*

## Analysis of Findings

Three active hosts were identified. The open ports reveal the services running and their operating system family (likely Windows for two hosts).

### Host 1: Windows Workstation/Server (192.***.***)

  * **135/tcp (msrpc):** Microsoft Remote Procedure Call.
  * **139/tcp (netbios-ssn):** NetBIOS Session Service (legacy file/print sharing).
  * **445/tcp (microsoft-ds):** Server Message Block (SMB) for modern file/printer sharing.
  * **2701/tcp (sms-rcinfo):** Likely a remote control/management application.
  * **Security Note:** The presence of **996 filtered ports** indicates a **local firewall** is active, silently dropping connection attempts to other ports.

### Host 2: DNS Server (192.***.***.\*\*\*)

  * **53/tcp (domain):** Domain Name System (DNS).
  * **Analysis:** This device is acting as a dedicated **DNS server** for the network.

### Host 3: Windows System with Unknown Service (192.***.***.\*\*)

  * **135/tcp, 139/tcp, 445/tcp:** Confirms a Windows operating system.
  * **6646/tcp (unknown):** This is a non-standard port. Further research would be required to identify the specific application or malware using it.

-----

## Security Risks Identified

The scan highlights critical areas of exposure, particularly concerning the hosts running Microsoft services:

1.  **SMB Exposure (Ports 139, 445):** These services are notorious targets for network attacks, including:

      * **Exploits:** Vulnerabilities like **EternalBlue** (WannaCry) exploit the SMB protocol.
      * **Credential Harvesting:** Attackers can attempt to guess or brute-force credentials to gain file system access.
      * **Recommendation:** These ports should be secured, either by applying the latest patches or by restricting access via a firewall to only necessary internal IP addresses.

2.  **Unknown Service (Port 6646):** Any unidentified open port presents an inherent risk as its function and patch status are unknown. It could be a legitimate, niche application or a persistence mechanism for malware.

3.  **DNS Server (Port 53):** While necessary, this service can be abused for **DNS amplification attacks** or **cache poisoning** if the server configuration is insecure.

## Key Takeaways

This task successfully demonstrated the use of Nmap for **network reconnaissance**. It confirmed the presence of Windows systems and a DNS server, exposing services that require careful security management to prevent unauthorized access or exploitation.
