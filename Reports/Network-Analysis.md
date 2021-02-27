# Network Analysis

## Time Thieves
At least two users on the network have been wasting time on YouTube. 
Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. 
So far, Security knows the following about these time thieves:
- They have set up an Active Directory network.
- They are constantly watching videos on YouTube.
- Their IP addresses are somewhere in the range 10.6.12.0/24.

We inspected traffic and found the following evidence:

**Domain Name of the User's Custom Site (1) & Domain Controller of the AD Network (2)**
1. frank-n-ted.com
2. 10.6.12.12
![Custom-Site](/Network-Analysis-Screenshots/Custom-Site-and-DC.png)

**Malware Downloaded to 10.6.12.203**
- june11.dll

![june11-dll](/Network-Analysis-Screenshots/june11-dll.png)

**Malware Species**
- Trojan Malware
![Malware-Species](/Network-Analysis-Screenshots/Malware-Species.png)

## Vulnerable Windows Machines

The Security team received reports of an infected Windows host on the network. They know the following:
- Machines in the network live in the range 172.16.4.0/24.
- The domain mind-hammer.net is associated with the infected computer.
- The DC for this network lives at 172.16.4.4 and is named Mind-Hammer-DC.
- The network has standard gateway and broadcast addresses.

We inspected traffic and found the following evidence:

**Infected Windows Machine Information**
- Hostname: ROTTERDAM-PC$
- IP address: 172.16.4.205
- MAC address: 00:59:07:b0:63:a4

![Host-Info](/Network-Analysis-Screenshots/Vulnerable-Windows-Host-Info.png)

**Infected Host Username**
- matthijs.devries

![Infected-Host-Username](/Network-Analysis-Screenshots/Infected-Windows-Username.png)

**IPs Used in the Infection Traffic**
- 172.16.4.205 (Destination IP)
- 31.7.62.214 (Source IP)
![Infection-Traffic-IPs](/Network-Analysis-Screenshots/Infection-Traffic-IPs.png)

## Illegal Downloads

IT was informed that some users are torrenting on the network. The Security team does not forbid the use of torrents for legitimate purposes, 
such as downloading operating systems. However, they have a strict policy against copyright infringement.

IT shared the following about the torrent activity:
- The machines using torrents live in the range 10.0.0.0/24 and are clients of an AD domain.
- The DC of this domain lives at 10.0.0.2 and is named DogOfTheYear-DC.
- The DC is associated with the domain dogoftheyear.net.

We inspected traffic and found the following evidence:

**Host Information**
- MAC address: 00:16:17:18:66:c8
- Windows username: elmer.blanco
- OS Version: Windows NT 10.0

![Illegal-Download-Username](/Network-Analysis-Screenshots/Illegal-Download-Username.png)


![Illegal-Download-OS-Version](/Network-Analysis-Screenshots/Illegal-Download-OS-Version.png)

**Downloaded Torrent File**
- An episode of Betty Boop called Rhythm on the Reservation

![Torrent-File](/Network-Analysis-Screenshots/Torrent-File.png)
