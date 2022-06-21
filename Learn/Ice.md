# [Ice](https://tryhackme.com/room/ice)
Deploy & hack into a Windows machine, exploiting a very poorly secured media server.
## 6/21/22

- Nmap 
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
