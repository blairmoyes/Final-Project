# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- Gateway
  - **Operating System**: Microsoft Windows 10 Pro v10.0.18363
  - **Purpose**: Network Gateway
  - **IP Address**: 192.168.1.1
- Kali VM
  - **Operating System**: Kali Linux 5.4.0
  - **Purpose**: Attacker VM
  - **IP Address**: 192.168.1.90
- Target 1
  - **Operating System**: Linux 3.16
  - **Purpose**: Vulnerable Web Server
  - **IP Address**: 192.168.1.110
- Target 2
  - **Operating System**: Linux 3.16
  - **Purpose**: Vulnerable Web Server
  - **IP Address**: 192.168.1.115
- ELK
  - **Operating System**: Ubuntu Linux (version not specified)
  - **Purpose**: Elastic Stack Host
  - **IP Address**: 192.168.1.100
- Capstone
  - **Operating System**: Linux Distro (not specified)
  - **Purpose**: Test Machine
  - **IP Address**: 192.168.1.105

### Description of Targets

The targets of this attack were: `Target 1 | 192.168.1.110` ` Target 2 | 192.168.1.115 `

Targets 1 & 2 are Apache web servers and have SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. 
  
### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

**Excessive HTTP Errors**

The Excessive HTTP Errors alert is implemented as follows:
  - **Metric**: HTTP Response Status Codes
  - **Threshold**: Above 400 for >5 minutes
  - **Vulnerability Mitigated**: Brute Force Attack, Dictionary Attacks, Web Object Enumeration 
  - **Reliability**: High reliability.

**HTTP Request Size Monitors**

The HTTP Request Size Monitors alert is implemented as follows:
  - **Metric**: HTTP Request Bytes
  - **Threshold**: Above 3500 bytes for >1 minute
  - **Vulnerability Mitigated**: HTTP Buffer Overflow 
  - **Reliability**: Low reliability, likely to generate false positives. A typical HTTP request header size is 700-800 bytes, which would equate to roughly five HTTP requests from the same user in one minute or less. This seems like pretty normal web traffic.

**CPU Usage Monitor**

The CPU Usage Monitor alert is implemented as follows:
  - **Metric**: System Process CPU percentage
  - **Threshold**: Over 0.5 (50%) for >5 minutes
  - **Vulnerability Mitigated**: Living Off the Land Attack
  - **Reliability**: Low reliablility in a typical host setting. Likely to generate false positives. A 50% threshold could be exceeded by any number of CPU-intensitve processes, many of which are not malicious. 

### Suggestions for Going Further: Target 1

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

- Vulnerability 1: Open/Unfiltered Ports 80 & 22
  - **Patch**: Configure firewall settings to filter all ports on network hosts.
  - **Why It Works**: Filtering host ports on the network will prevent network scans from tools such as nmap, providing malicious actors with fewer attack vectors.

- Vulnerability 2: WordPress Enumeration
  - **Patch**: Install a WPScan blocker plug-in for your WordPress site. For more robust protection, install a more comprehensive
  WP security plug-in such as WP Security Optimizer.
  - **Why It Works**: The block-wpscan plugin will prevent WPScan from enumerating items of interest on your WordPress URL. The WP Security Optimizer plug-in will also elude scans in addition to blocking malicious User-Agents and performing file integrity checks.

- Vulnerability 3: Weak User Credentials
  - **Patch**: Implement a complex password policy. Passwords should contain a minimum of 12 characters containing upper case letters
  and at least one special character.
  - **Why It Works**: Complex passwords are less susceptible to password guessing, password cracking, and brute force attacks. No password is uncrackable, but strong passwords make them an impractical attack vector for malicious actors.

- Vulnerability 4: Misconfigured File Privileges on Target Systems
  - **Patch**: Implement the principle of least privilege. Create role-based groups on the target system with specialized privileges that only give users the access they need to do their job. You may add or remove users to groups as needed, but avoid assigning file privileges to individual users.
  - **Why It Works**: When implemented properly, the principle of least privilege ensures restrictive, but easy-to-manage file system access for users. Properly configured user privileges also limit an attacker's opportunities when inside a system.

- Vulnerability 5: Brute Force Attack
  - **Patch**: Implement a password lockout policy. Use device cookies to discern between known and unknown devices. Implement CAPTCHAs on login pages.
  - **Why It Works**: A lockout policy that can discern between known and unknown devices will effectively block brute force attacks while also minimizing susceptibility to DoS attacks.

- Vulnerability 5: Privilege Escalation via Misconfigured Python Library
  - **Patch**: Harden privileges for the /usr/bin/python directory by revoking write & execute access for non-administrative users. Reconfigure local sudoers file to prevent non-administrative users from executing commands in the /usr/bin/python directory as root.
  - **Why It Works**: Restricting sudo privileges in the /usr/bin/python directory will prevent compromised non-administrative accounts from running Python commands out of root.

  ### Suggestions for Going Further: Target 2

  - Vulnerability 1: Open/Unfiltered Ports 80 & 22
  - **Patch**: Configure firewall settings to filter all ports on network hosts.
  - **Why It Works**: Filtering host ports on the network will prevent network scans from tools such as nmap, providing malicious actors with fewer attack vectors.

- Vulnerability 2: Web Object Enumeration
  - **Patch**: Configure firewall settings to auto-drop traffic from a given IP address generating excessive HTTP 400-Class response codes in a short period of time.
  - **Why It Works**: Web object enumeration is typically carried out via brute force attack against the target URL, generating many HTTP 400-class response codes. Dropping traffic from IP addresses that generate too many is a simple and effective method of blocking web object enumeraiton.

- Vulnerability 3: PHP File Upload
  - **Patch**: Configure firewall settings to block uploads containing the ".php" extension. Scramble file names and extensions. Require authentication for file uploads. Limit user privileges for file uploads.
  - **Why It Works**: Combining these configuration rules provides a robust defense against PHP file uploads at multiple layers.
