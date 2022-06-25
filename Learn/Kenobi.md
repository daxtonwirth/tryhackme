# [Kenobi](https://tryhackme.com/room/kenobi)
Walkthrough on exploiting a Linux machine. Enumerate Samba for shares, manipulate a vulnerable version of proftpd and escalate your privileges with path variable manipulation.

6/25/22

## Summary:
- [Enumerate machine smb shares](#enumeration)
- [Exploit vulnerable Proftpd version](#exploit-proftpd)
- [Excalate privileges with SUID](#privesc)

## Enumeration
- Nmap

![image](https://user-images.githubusercontent.com/66894542/175762001-06ece29c-1826-4b77-9e4f-a62ae27e0299.png)

- To enumerate the open smb ports, we can use smb nmap scripts:
- `nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.169.20`

![image](https://user-images.githubusercontent.com/66894542/175762079-f66a9c2e-9e84-4953-9027-7b426c68dda2.png)

- To quickly get the files on the server with the anonymous user, we can use the `smbget` command:
- `smbget -R smb://10.10.169.20/anonymous`

![image](https://user-images.githubusercontent.com/66894542/175762115-40e4a16f-f807-4238-b3c6-c34e38f1c756.png)

- We can also further enumerate port 111 having rpcbing:
- `nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.169.20`

![image](https://user-images.githubusercontent.com/66894542/175762231-cfc1efb5-d64d-41ae-a2cb-0a18a2a1d480.png)

- We can see the /var mount here

## Exploit ProFTPd
- We can do a banner grab of the ftp service with netcat which shows us the version:
- `nc 10.10.169.20 21`

![image](https://user-images.githubusercontent.com/66894542/175762312-2650a35c-4eb6-4413-83fa-a4f9d758d6f8.png)

- With searchsploit, we can find a LOT of vulnerable versions:

![image](https://user-images.githubusercontent.com/66894542/175762406-e1c30093-6c99-4544-aa75-81dec7e075e7.png)

- We can scroll down and find our version:

![image](https://user-images.githubusercontent.com/66894542/175762476-57a7b08a-fea7-4471-8f80-e681cbdab801.png)

- Due to misconfiguration, we can find an rsa ssh key and copy it to somewhere we can see: the /var directory which we found previously:

![image](https://user-images.githubusercontent.com/66894542/175762540-62db6896-ca7c-473e-a2ad-7fe999182c1d.png)

- Now we can mount the /var directory to our system to get access to the ssh keys:

![image](https://user-images.githubusercontent.com/66894542/175762620-c7123a8d-0a91-410b-82ad-64602175c94f.png)

- Now we can ssh into the machine!

![image](https://user-images.githubusercontent.com/66894542/175762655-0c7f297b-a8d5-408d-a2c5-0ac56e6ceced.png)

## PrivEsc
- For this room we are going to try and find a SUID bit file we can use for privilege escalation:
`find / -perm -u=s -type f 2>/dev/null`

![image](https://user-images.githubusercontent.com/66894542/175762776-6ea90f33-4c32-4fac-814f-839891317d36.png)

- Look for files that stand out and are not supposed to be there. [GTFObins](https://gtfobins.github.io/#) can help out (search and look for SUID)
- The menu file is suspicious so lets look into it more.
- Because the program uses commands like curl (using `strings` to find these) without an absolute path, we can make our own curl file with `/bin/sh` to get a shell as the root user
- Now we just need to add the directory with our curl command in our PATH so it runs it

![image](https://user-images.githubusercontent.com/66894542/175763049-a399bedf-6f38-4dae-9c91-af51fbb07a7f.png)

