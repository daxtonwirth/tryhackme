# Windows Forensics 1
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

- Here is where recent files were opened:
![image](https://user-images.githubusercontent.com/66894542/174453062-756202b3-005d-44f9-88dc-8d52eb82d7b9.png)

- See the full path of the python executable:
![image](https://user-images.githubusercontent.com/66894542/174453164-fe647c93-3174-44c7-8d8e-4d0f44fa113f.png)
The locations of the files are hard to remember so a useful cheatsheet is included!
![image](https://user-images.githubusercontent.com/66894542/174453553-6426cbed-0b6d-4f08-8753-4bfc34beb3d1.png)
