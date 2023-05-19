## Scanning with Nmap
- For help "nmap --help"
- nmap -sV --script vuln
	- To find vulnerabilities.
- nmap -T4 -A -p- 
	- -T4 - This is for the speed of the scan, the bigger the faster but more detectable by the host.
	- -A - Scan everything. Version, operating system, finger printing...
	- -p- - Scan all ports. Without this will scan the top 1000 ports. For specific ports, -p 443,80.
- nmap -sU -T4 -p
	- Scan UDP -sU
## Enumerating HTTP and HTTPS
- Fist of all if a webserver is running go on the internet and search for the ip address and investigate website while taking notes of the findings.
- On the website look at the source code and look for notes, information disclosres, usernames or passwords.
- We can use a tool called "Nikto" to enumerate features of the webserver. - nikto -h http://ipaddress
- After this you can use dirbuster to enumerate directories inside the webserver. Run dirbuster& on terminal and it will be open.
	- Specify the target url with the port > go faster > list based brute force > /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt > file extention depending on what the server is run with, php is the base for apache, but more examples php,txt,zip,rar,pdf
	- There are more files to use, depends on what you want. 
- Another way to find directories is using ffuf:
	- ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://192.168.138.136/FUZZ

## Enumerating SMB
- SMB is a file share. Used on a network to share files and folders.
-  auxiliary/scanner/smb/smb_version -  in metasploit. Use this to find the version of SMB on the machine. 
- smbclient - smbclient -L <n>\\\\192.168.138.131\\ </n> - trys to connect to the SMB and list the files.
	- ![[Pasted image 20230418160732.png]]
	- We were able to list 2 Sharenames. We can try to connect to it by using. smbclient -L <n>\\\\192.168.138.131\\IPC$ </n>
	- We actually get access to IPC but we are not able to list or do anything inside. 
## Using Metasploit

## Researching Potential Vulnerabilities
- Go through the notes and search online for exploits with the versions. Example "Samba 2.2.1a exploit". Note the finsing on the vulnerabilities file. 
- Also can use "searchsploit Samba 2.2". do not be too specific when using searchsploit