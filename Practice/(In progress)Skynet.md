# [Skynet](https://tryhackme.com/room/skynet)
A vulnerable Terminator themed Linux machine.

6/27/22

```
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
|_  256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (EdDSA)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Skynet
110/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: SASL PIPELINING TOP AUTH-RESP-CODE CAPA RESP-CODES UIDL
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
|_imap-capabilities: more LOGINDISABLEDA0001 IDLE capabilities ENABLE have Pre-login LOGIN-REFERRALS post-login IMAP4rev1 ID LITERAL+ SASL-IR listed OK
445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
MAC Address: 02:7C:D9:CB:10:11 (Unknown)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: SKYNET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2022-06-27T16:55:09-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2022-06-27 22:55:09
|_  start_date: 1600-12-31 23:58:45
```


These are the directories I found with gobuster:
```
/.hta (Status: 403)
/.htpasswd (Status: 403)
/admin (Status: 301)
/.htaccess (Status: 403)
/config (Status: 301)
/css (Status: 301)
/index.html (Status: 200)
/js (Status: 301)
/server-status (Status: 403)
/squirrelmail (Status: 301)
/ai (Status: 301)
```

I found squirrelmail at 
![image](https://user-images.githubusercontent.com/66894542/176042965-9af3a71e-cf6b-4571-a237-5ae6262d2948.png)

From enum4 linux I found the following: 
milesdyson- Miles Dyson Personal Share
anonymous- Skynet Anonymous Share

- I connected to the anonymous share
- `smbclient  '\\10.10.184.106\anonymous'`

#### SMB files
attention.txt
`A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this.
-Miles Dyson`

log1.txt: 

`cyborg007haloterminator
terminator22596
terminator219
terminator20
terminator1989
terminator1988
terminator168
terminator16
terminator143
terminator13
terminator123!@#
terminator1056
terminator101
terminator10
terminator02
terminator00
roboterminator
pongterminator
manasturcaluterminator
exterminator95
exterminator200
dterminator
djxterminator
dexterminator
determinator
cyborg007haloterminator
avsterminator
alonsoterminator
Walterminator
79terminator6
1996terminator`

This appears to be a password list (due to special characters so lets try some of these passwords)

The first one worked! Our first email looks interesting:

![image](https://user-images.githubusercontent.com/66894542/176065869-a2c86dfe-e001-44f4-905e-cf37b712388e.png)

We also have this weird encoded message in his inbox:
![image](https://user-images.githubusercontent.com/66894542/176066664-b40ce327-9f1e-46a3-8d57-0a8cc6232c94.png)
Which decode to this:
`balls have zero to me to me to me to me to me to me to me to me to`
Which is similar to this one:
![image](https://user-images.githubusercontent.com/66894542/176066706-689ce617-2ea3-401e-8946-0b4f6db55c27.png)

I did a quick [search]([https://languagelog.ldc.upenn.edu/nll/?p=33355](https://en.wikipedia.org/wiki/Language_creation_in_artificial_intelligence)) and found that computers made up this language at Facebook

## Using found email creds to login to smb
Lets try to log into milesdyson smb share now: 
![image](https://user-images.githubusercontent.com/66894542/176068458-8cadd12f-eded-4d0b-ba3d-7ebef60be199.png)

There were a lot of files but the one that stuck out the most was the one in the notes directory called important.txt that has a cms directory!
![image](https://user-images.githubusercontent.com/66894542/176068788-d3cb799d-5f79-4505-affc-1a42b68baa34.png)

![image](https://user-images.githubusercontent.com/66894542/176069036-37b0d4de-2190-4ebf-b169-db83d4545b4c.png)

Doing a quick gobuster on it, I found an administrator page:
```
/.hta (Status: 403)
/.htpasswd (Status: 403)
/administrator (Status: 301)
/.htaccess (Status: 403)
/index.html (Status: 200)
```

And here we have a potentially vulnerable service:
![image](https://user-images.githubusercontent.com/66894542/176069178-13c50c65-6916-404b-9252-2c402cbe679f.png)



