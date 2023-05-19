Interest:
- Port 80 :
	- OpenSSH 7.9p1 Debian 10+deb10u2 
	- _http-title: Bolt - Installation error 
	- http-server-header: Apache/2.4.38 (Debian)
- Port 111:
	- open  rpcbind 2-4 (RPC #100000)
- Port 2049:
	- open  nfs_acl 3 (RPC #100227)
- Port 8080Ç
	- Apache httpd 2.4.38
	- PHP 7.3.27-1~deb10u1 - phpinfo()


─$ nmap -T4 -A -p- 192.168.0.35
Starting Nmap 7.93 ( https://nmap.org ) at 2023-05-12 12:31 BST
Nmap scan report for 192.168.0.35
Host is up (0.0098s latency).
Not shown: 65530 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 bd96ec082fb1ea06cafc468a7e8ae355 (RSA)
|   256 56323b9f482de07e1bdf20f80360565e (ECDSA)
|_  256 95dd20ee6f01b6e1432e3cf438035b36 (ED25519)
80/tcp   open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Bolt - Installation error
|_http-server-header: Apache/2.4.38 (Debian)
111/tcp  open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      37549/udp6  mountd
|   100005  1,2,3      44000/udp   mountd
|   100005  1,2,3      49079/tcp   mountd
|   100005  1,2,3      53201/tcp6  mountd
|   100021  1,3,4      34058/udp6  nlockmgr
|   100021  1,3,4      38393/tcp   nlockmgr
|   100021  1,3,4      42291/tcp6  nlockmgr
|   100021  1,3,4      50110/udp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp open  nfs_acl 3 (RPC #100227)
8080/tcp open  http    Apache httpd 2.4.38 ((Debian))
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
