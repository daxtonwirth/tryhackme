# [Windows Forensics 1](https://tryhackme.com/room/windowsforensics1)
## 6/18/22

- Artifacts- pieces of info that provide evidence of human activity
- “improve experience” for the user

### Registry
- regedit.exe
- access hives offline 
- amcache.hve- programs last executed
- Tools: KAPE (live), autopsy (both), ftk imager (both)
- Registry tools: regRipper, Zimmerman's Registry Explorer
- Shellbags- information on a users window view (like adjusting windows explorer)- persist even if file is securely deleted
- Use KAPE to search for Registry hives and check them
![image](https://user-images.githubusercontent.com/66894542/174899652-55ae58c8-d147-4a0a-ad89-f3573a8ca9b5.png)
- Find the SAM hive
![image](https://user-images.githubusercontent.com/66894542/174899728-92938ec1-5060-4a9e-8644-b965fb374cf8.png)
- Load the SAM hive in Registry explorer and find the Users folder in ROOT/SAM/Domains/Account/Users:
![image](https://user-images.githubusercontent.com/66894542/174899787-963f5420-78c7-4637-a0e5-6344a321960c.png)


- Here is where recent files were opened:
![image](https://user-images.githubusercontent.com/66894542/174453062-756202b3-005d-44f9-88dc-8d52eb82d7b9.png)

- See the full path of the python executable:
![image](https://user-images.githubusercontent.com/66894542/174453164-fe647c93-3174-44c7-8d8e-4d0f44fa113f.png)
The locations of the files are hard to remember so a useful cheatsheet is included!
![image](https://user-images.githubusercontent.com/66894542/174453553-6426cbed-0b6d-4f08-8753-4bfc34beb3d1.png)
