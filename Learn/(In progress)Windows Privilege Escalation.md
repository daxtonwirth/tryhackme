# Windows Privilege Escalation
## 6/17/22

- Users < Administrators < System (cant use like regular account though)
- Other possible privesc accounts: local service, network service 
### Easiest: find creds in plain text on system
- Windows Deployment Services: Unattended windows installation that has admin account creds possibly stored in different locations like 
  - C:\Unattend.xml
  - C:\Windows\Panther\Unattend\Unattend.xml
  - C:\Windows\system32\sysprep.inf
  - C:\Windows\system32\sysprep\sysprep.xml
- Powershell history: passwords could be part of command line argument
![image](https://user-images.githubusercontent.com/66894542/174423755-5e42faad-7172-4b0d-9afd-6bab27b07ad1.png)
- Saved Windows Creds: `cmdkey /list` to list and then `runas /savecred /user:admin cmd.exe` to see them
![image](https://user-images.githubusercontent.com/66894542/174424004-f2296c4a-3596-4787-b3a5-fae81a239ee2.png)

- IIS config: `web.config` stores passwords for databases or authentication mechanisms loacated at
  - C:\inetpub\wwwroot\web.config
  - C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
  - To find: `type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString`
![image](https://user-images.githubusercontent.com/66894542/174423833-1a73f4bc-997c-451d-a15c-7009e95df5e0.png)

- Putty stores sessions that have cleartext authentication credentials
  -   `reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "ProxyPassword" /s`
![image](https://user-images.githubusercontent.com/66894542/174424026-8ccdb193-bfcc-47fa-bfa8-e14f0f62cd0b.png)

## Other Quick methods
- Scheduled tasks (like cron jobs for linux) where we can either modify the file or have permissions for that file
  - `schtasks /query /tn TASK_NAME /fo list /v`  
  - `icacls` to check file permissions
![image](https://user-images.githubusercontent.com/66894542/174424250-ac421aee-85a1-47a4-b6e3-07adc85bd6d9.png)
![image](https://user-images.githubusercontent.com/66894542/174424268-d7207371-f8eb-441c-bbaf-8850fa8f643f.png)
- Then ran into an error where it was not connecting which other users where struggling with as well:
![image](https://user-images.githubusercontent.com/66894542/174424998-24ee3fb0-1d25-4d1e-9e89-a61d14418239.png)
