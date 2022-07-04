# [Mustacchio](https://tryhackme.com/room/mustacchio)
Easy boot2root Machine

7/4/22

## Enumeration
Nmap

```
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 58:1b:0c:0f:fa:cf:05:be:4c:c0:7a:f1:f1:88:61:1c (RSA)
|   256 3c:fc:e8:a3:7e:03:9a:30:2c:77:e0:0a:1c:e4:52:e6 (ECDSA)
|_  256 9d:59:c6:c7:79:c5:54:c4:1d:aa:e4:d1:84:71:01:92 (EdDSA)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Mustacchio | Home
MAC Address: 02:C2:99:FA:11:7B (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![image](https://user-images.githubusercontent.com/66894542/177209273-ada55895-7691-4ca3-9185-f1a0655d0016.png)

Gobuster:
`gobuster dir -u http://10.10.213.107 -w /usr/share/wordlists/dirb/common.txt`

```
/custom (Status: 301)
/fonts (Status: 301)
/images (Status: 301)
/index.html (Status: 200)
/robots.txt (Status: 200)
/server-status (Status: 403)
```

![image](https://user-images.githubusercontent.com/66894542/177209470-32dca1b5-f924-47d5-bb5e-2bfdbe8ffdad.png)

`file users.bak`

`users.bak: SQLite 3.x database, last written using SQLite version 3034001`

```
sqlite> .tables
users
sqlite> select * from users;
admin|1868e36a6d2b17d4c2745f1659433a54d4bc5f4b
```

I looked up the hash online and we got a password! Next I tried to login with ssh but it was denied due to not having a public key. 
I looked on the website for a login page and could not find one so back to enumeration!

I did the top 5000 ports and we found another one!
`nmap --top-ports 5000 10.10.213.107`

```
22/tcp   open  ssh
80/tcp   open  http
8765/tcp open  ultraseek-http
```


## Exploitation
This web server has a admin login which we can login to with our creds. It leads to a comment page. 
I tried to post a comment but nothing was showing up so I looked at the source and found out it has to be in xml format 
by looking at the example at "/auth/dontforget.bak" and the alert to "Insert XML Code!"

With this we can try XXE (XML external entity) injection. I found payloads [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection#detect-the-vulnerability)

![image](https://user-images.githubusercontent.com/66894542/177212639-d2f47c57-bfb1-4bda-93bf-8fd6119aafdb.png)

This key is encrypted though so we need to find the key with john

`/opt/john/ssh2john.py barry > hash`

Then we can login through ssh!

## PrivEsc
`find / -type f -perm -u=s 2>/dev/null`

Here we find an unusual file which is an elf binary:
/home/joe/live_log

Looking at the strings of the file, we see the tail command run without the full path, so we can easily exploit this by creating our own tail file with the contents /bin/bash and then run it!



