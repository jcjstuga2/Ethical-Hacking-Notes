ip: 192.168.0.35

Interesting:
- http://192.168.0.35/README.md
- Search bolt on msfconsole (Bolt is mostlikelly being used on the port 80)
- port 80 - app/config/configuration.yml
	- database:
	    driver: sqlite
	    databasename: bolt
	    username: bolt
	    password: I_love_java

- BoltWire exploit result:
	- root:x:0:0:root:/root:/bin/bash
	daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
	bin:x:2:2:bin:/bin:/usr/sbin/nologin
	sys:x:3:3:sys:/dev:/usr/sbin/nologin
	sync:x:4:65534:sync:/bin:/bin/sync
	games:x:5:60:games:/usr/games:/usr/sbin/nologin
	man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
	lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
	mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
	news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
	uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
	proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
	www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
	backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
	list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
	irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
	gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
	nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
	_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
	systemd-timesync:x:101:102:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
	systemd-network:x:102:103:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
	systemd-resolve:x:103:104:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
	messagebus:x:104:110::/nonexistent:/usr/sbin/nologin
	sshd:x:105:65534::/run/sshd:/usr/sbin/nologin
	jeanpaul:x:1000:1000:jeanpaul,,,:/home/jeanpaul:/bin/bash
	systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
	mysql:x:106:113:MySQL Server,,,:/nonexistent:/bin/false
	_rpc:x:107:65534::/run/rpcbind:/usr/sbin/nologin
	statd:x:108:65534::/var/lib/nfs:/usr/sbin/nologin