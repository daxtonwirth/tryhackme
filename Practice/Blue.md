# [Blue](https://tryhackme.com/room/blue)
## 6/18/22
- IP: 10.10.166.178

### Nmap scan
![image](https://user-images.githubusercontent.com/66894542/174460186-d94ccf98-79c8-4b3d-a24b-7465cdadda11.png)
- Vulnerable to ms17-010 or cve-2017-0144
  - use exploit/windows/smb/ms17_010_eternalblue in metasploit
![image](https://user-images.githubusercontent.com/66894542/174460239-892a35a4-43af-47b4-b324-ac20b663b850.png)
- Dump users password hashes with hashdump
![image](https://user-images.githubusercontent.com/66894542/174460476-544a545f-ca54-4936-a87c-e3a177191da3.png)
- The steps are pretty self explanatory
