# Game Zone
## 6/18/22
- Ip: 10.10.212.224

- First, perform a nmap scan on the ip:
![image](https://user-images.githubusercontent.com/66894542/174456086-ef8afb69-7e4d-45fa-a999-93ab2806499c.png)
- SSH does not look vulnerable so lets go to the website.
- For the first question, I did a quick reverse image search and found it was from hitman (I then searched for hitman character names):
![image](https://user-images.githubusercontent.com/66894542/174455621-70831d7c-19b0-49ea-a425-832b21b0e4f3.png)
- Next is a simple SQL injection:
![image](https://user-images.githubusercontent.com/66894542/174455719-b72cb28d-f126-47ff-9e6b-1c17a43b74c3.png)
- To automate our SQLi, we can use SQLmap. First we need to catch the request using burp:
![image](https://user-images.githubusercontent.com/66894542/174455807-5f847107-fccc-4f31-857a-66e8412e248c.png)
- Then put this into a file and exploit:
![image](https://user-images.githubusercontent.com/66894542/174455847-ad85d3aa-46ae-4dae-a8df-85f363406a5e.png)
- Then it will find the user table and hashed password:
![image](https://user-images.githubusercontent.com/66894542/174455950-1f3bb164-da9d-4393-b5ff-413993fba414.png)
- Then we can crack the hash with John the ripper:
![image](https://user-images.githubusercontent.com/66894542/174456041-73b25c34-6e5b-4b18-b23d-cb4df82e3599.png)
- Next lets login!
![image](https://user-images.githubusercontent.com/66894542/174456129-6fae4572-ad3a-4cd0-a984-42102bc69a58.png)
- We found a port listening on the server that was blocked externally by the firewall so we used a reverse ssh tunnel to get past this and access the port
- `ssh -L 10000:localhost:10000 <username>@<ip>`
![image](https://user-images.githubusercontent.com/66894542/174456253-acaeb6e7-279f-40d7-a1a9-399e6706efee.png)
- We can then login with agent47:
![image](https://user-images.githubusercontent.com/66894542/174456296-89b0f893-1f3d-4e24-8dbf-a4d79dbb9898.png)
- I did a quick search online and found this:
![image](https://user-images.githubusercontent.com/66894542/174456542-0b8df014-a0ea-422b-879b-532fca270ac9.png)
- Set the options:
![image](https://user-images.githubusercontent.com/66894542/174456959-12755790-d9e9-4b35-bd64-c0d35c1614a9.png)

- Exploit:
![image](https://user-images.githubusercontent.com/66894542/174456749-90c71b76-3353-4c05-8f06-dcc8c6638130.png)
