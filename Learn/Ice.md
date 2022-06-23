# [Ice](https://tryhackme.com/room/ice)
Deploy & hack into a Windows machine, exploiting a very poorly secured media server.

6/21/22

## Summary:
- Find a icecast metasploit module for initial access
- Elevate privilege also with metasploit
- Explore post-exploitation options such as screenshare


Nmap 
  - nmap -sS 10.10.183.106 -p- -sV -sC
  - nmap 10.10.183.106 (to save time)

![image](https://user-images.githubusercontent.com/66894542/174902791-389fee0f-c98e-4007-9c24-e1ce24afbd71.png)


- To get more info on a specific service, we can target that service to save time:

![image](https://user-images.githubusercontent.com/66894542/174902368-31152cb8-c568-4412-8698-fbcaf96762b3.png)

- With the -sC flag, we can see the following:

![image](https://user-images.githubusercontent.com/66894542/174903441-90becf61-3597-44a4-ab39-23bb89466e8a.png)

- Now, we can dig into some of this info we found, starting with Icecast and [I found many CVEs](https://www.cvedetails.com/vulnerability-list/vendor_id-693/Icecast.html)

![image](https://user-images.githubusercontent.com/66894542/174904097-454be20f-fc71-405c-a54d-297460203b33.png)

- Then we can use our meterpreter to run commands, such as sysinfo:

![image](https://user-images.githubusercontent.com/66894542/174904912-3fc90fa7-7f33-4cba-994b-20e9e6b9ff94.png)

- We can try the post/multi/recon/local_exploit_suggester module to find more exploits to elevate our privileges:

![image](https://user-images.githubusercontent.com/66894542/174905291-adbc4e31-793a-415f-81a1-bbe6553b7b9b.png)

- Now we can check our priveleges:

![image](https://user-images.githubusercontent.com/66894542/174905693-a96ef45b-e444-4082-a83d-e77b8d9b9ea1.png)

- Now we can migrate to the printer spool process that has the same permissions with the command `migrate -N PROCESS_NAME`

![image](https://user-images.githubusercontent.com/66894542/175388581-f7761383-a56b-4594-9806-1b447e4d5109.png)

- This gives us system!

![image](https://user-images.githubusercontent.com/66894542/175389307-62dc3617-a2ec-4cc6-8e1f-fbd6f4ed0ad0.png)

- Then we can load mimikatz (kiwi) to dump the credentials and we find Dark's password (a scheduled task is running on the system which is why we can do this and defender is not enabled)

![image](https://user-images.githubusercontent.com/66894542/175390000-fee22ec9-a089-4ddd-9dba-78c5d406acb0.png)

- Then we can perform post-exploitation such as screenshare:

![image](https://user-images.githubusercontent.com/66894542/175393386-41fe281c-287b-4e10-bbc8-c6ba2156d6fc.png)

