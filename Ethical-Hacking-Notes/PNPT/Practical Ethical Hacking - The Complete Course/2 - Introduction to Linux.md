## Usefull Commands
https://explainshell.com/ - Use for explaining any command.  
<u>passwd</u> - new password
<u>man "comand"</u> - Will list the manual about that command
<u>pwd</u> - Locations of the current direction
<u>cat "file name"</u>- display text from a file
<u>ls</u> - Folders/file on the directory
<u>ls -la</u> - List all the files
<u>mkdir  "Name"</u> - Create folder/directory
<u>rmdir "Name"</u> - Remore folder/directory
<u>echo "Hi!" > test.txt</u> - Creates a file with "Hi!". Using > overwrites, using >> adds to the file. 
<u>touch "file name"</u> - Creates a new file.
<u>nano "file name"</u> - Edit/updating the file. If the "file name" is not existent will create a new file.
<u>mousepad "file name"</u> - Edit/updating the file with a graphical interface. If the "file name" is not existent will create a new file.
<u>cp "file name" "directory"</u>  - copy the file to a new directory.
<u>rm "file name"</u> - delete the file, define path if needed.
<u>mv "file name" "directory"</u> - Move the file from the directory to a new one.
<u>locate "file name"</u> - returns the file location (use sudo updateedb to get the new files created)
<u>chmod +rwx "file name"</u> - changes the privelages on a file for the owner. Variants can be ---,r-x or --x .
<u>chmod 777 "file name"</u> - changes the privales for all groups to rwx.
![[Pasted image 20230408201422.png]]
<u>Sudo adduser "name"</u> - Creates a new user.
<u>Su "user name"</u> - Switch to the user name defined. 
<u>grep  "sudo" directory</u> - This will look for the string "sudo" in the directory. 
### Networking commands
<u>ip a</u> -  new way of displaying ip data
<u>ip n or arp -a</u> - Connects ip with mac address. IP Neighbour.
<u>arp-scan -l</u> - Scans the ip addresses in your home network.
<u>ip r or route</u> -  Routing table, where the trafic is coming from in a network. For reference: https://www.redhat.com/sysadmin/route-ip-route

<u>netstat</u> - Reference: https://www.redhat.com/sysadmin/netstat#:~:text=The%20network%20statistics%20(%20netstat%20)%20command,common%20uses%20for%20this%20command.

### Services commands
<u>sudo service apache2 start</u> - Starts a apache2 server using our machin's ip address. The location of the hosted file is /var/www/html/index.html. This can be used to host a malitious exploit/code. 
<u>sudo service apache2 stop</u> - stops the server. 
<u>python3 -m http.server "port number (exmpl:80)"</u> - creates a server, hosting all the files on the directory. Will create a server on the directury it is ran and enables other computers to download files from that directy using 
***wget 10.10.69.247:8000/file***.
<u>sudo systemctl enable "service name (exmpl: ssh)"</u> - This will enable the service to start when the machine is started. 

### Installing and Updating Tools
<u>sudo apt update && apt upgrade</u> - This will update and upgrade the machine. 
<u>sudo apt install "name"</u> - Install the tool specified. 
 

### Scripting with Bash
<u>ping 192.168.183.1 -c 1 > ip.txt</u> - This will save the resposnse from this comand on the ip.txt

<u>cat ip.txt | grep "64 bytes" | cut -d " " -f 4 | tr -d ":"</u> - this will return the ip address. Grep will look for the line with "64 bytes", cut will get the text from the 3 space until the 4. and tr will remove the ":".

Exercise example: 
1 - Create a file called "ipsweep.sh"
2 - Inside include:
#!/bin/bash if [ "$1" == "" ] then echo "You forgot an IP address!" echo "Syntax: ./ipsweep.sh 192.168.1" else for ip in `seq 1 254`; do ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" & done fi
3 - run: <u>./ipsweep.sh 192.168.183 > ip.txt</u>
4 - run: <u>for ip in $(cat ip.txt); do nmap $ip; done</u>








<u>ctrl + L</u> - Clear the terminal


## User and Privilages
When usinf ls -la we will have the infomration about the previlages for a file/folder

r - read
w - write
x - execute
1 - File (f), Directory (d)
2 - Owner/Creator group 
3 - Group ownership
4 - All other users group
1  2     3     4
d rwx rwx rwx
drwxr-xr-x
frwxr-xr--

