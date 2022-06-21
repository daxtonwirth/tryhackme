# [Attacktive Directory](https://tryhackme.com/room/attacktivedirectory)
## 6/20/22

- Nmap
![image](https://user-images.githubusercontent.com/66894542/174670952-c51ba729-3bd8-4c05-89fd-9e2f05156af4.png)

- I cloned https://github.com/ropnop/kerbrute and made the file to run: 
![Inked174674161-fea8ce67-79ff-4cf5-8eb1-c52d50a796ce](https://user-images.githubusercontent.com/66894542/174675638-134eba35-1aab-4766-91dc-b43da7ee1b2a.jpg)

- Looking on [hashcat](https://hashcat.net/wiki/doku.php?id=example_hashes), we can find the name of this hash, along with the mode in the first column: 
![image](https://user-images.githubusercontent.com/66894542/174675936-f3321f73-35e8-49a2-aa78-b787ad968273.png)

- I then started hashcat with this info:
![image](https://user-images.githubusercontent.com/66894542/174676480-127e4700-0acb-46c3-a66d-0ac38c58d46b.png)

- Next we can use these credentials to enumerate shares:
![image](https://user-images.githubusercontent.com/66894542/174677964-16edb973-2b19-4f8b-9ed7-2516cfb60ab9.png)

- Lets get more info about the backup share that looks interesting:
![image](https://user-images.githubusercontent.com/66894542/174678579-3b7df01e-2763-4533-84a5-d3718db46432.png)

- Download the file to our machine:
![image](https://user-images.githubusercontent.com/66894542/174678527-9676c515-4724-41a6-b957-17ad911f04c7.png)

- Then decode the contents:
- ![image](https://user-images.githubusercontent.com/66894542/174678821-e23fa174-4c44-41ff-bea9-a25138767c64.png)

- Use these credentials to get the hashes with a tool in Impacket called "secretsdump.py":
![image](https://user-images.githubusercontent.com/66894542/174679491-78940f3d-c1ab-4979-afd9-57da5f1bedc3.png)

- Then we can try a pass the hash attack and send the hash as the users password with evil-winrm:
![image](https://user-images.githubusercontent.com/66894542/174699367-7e817be5-c3ab-42c0-9167-76031d2d22bf.png)
