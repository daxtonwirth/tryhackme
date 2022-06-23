# [Splunk](https://tryhackme.com/room/bpsplunk)
Part of the Blue Primer series, learn how to use Splunk to search through massive amounts of information

6/23/22

### What IP is scanning our web server?
- In order to discover what ip was doing a port scan, I first found the correct index and then looked at the rare values with the dest_port and found consecutive ports each with a count of 1:
`index=botsv1 imreallynotbatman.com| rare limit=50 dest_port  src_ip `

![image](https://user-images.githubusercontent.com/66894542/175423759-efcf1f1d-c152-44b7-81bb-506a0d1229c8.png)

### What web scanner scanned the server?
- We can find this in the source header:
![image](https://user-images.githubusercontent.com/66894542/175426973-a2d7e5c1-cb2c-4959-aa9b-3f92404a6e46.png)

### What content management system is imreallynotbatman.com using?
- We can find this in the destination header:
![image](https://user-images.githubusercontent.com/66894542/175427156-50187671-0cc1-45ed-896d-cb2f8e9526c3.png)

### What address is performing the brute-forcing attack against our website?
- For this one, I first looked at the http methods used:
![image](https://user-images.githubusercontent.com/66894542/175427585-46c4e905-0a50-4589-8e70-d2b7ac604c99.png)
- For a brute force attack, post is usually used so I refined my search. Now we need to determine how to see data for the brute force attack. 
- Let's first look at the form_data and then use some regex to get only the ones with username and password in them which we can see is pretty obvious:
![image](https://user-images.githubusercontent.com/66894542/175428650-03876020-4f55-4f2e-8fb6-7ad93d61dc5e.png)

### What was the first password attempted in the attack?
- Now we just need to sort by time to quickly find it:
```index=botsv1 sourcetype=stream:http dest="192.168.250.70" http_method=POST form_data=*username*passwd* src="23.22.63.114" 
| sort by _time 
| head 1
```

### What was the correct password for admin access to the content management system running imreallynotbatman.com?

### What was the average password length used in the password brute forcing attempt rounded to closest whole integer?

### How many seconds elapsed between the time the brute force password scan identified the correct password and the compromised login rounded to 2 decimal places?

### How many unique passwords were attempted in the brute force attempt?

### What is the name of the executable uploaded by P01s0n1vy?

### What is the MD5 hash of the executable uploaded?

### What is the name of the file that defaced the imreallynotbatman.com website?

### This attack used dynamic DNS to resolve to the malicious IP. What fully qualified domain name (FQDN) is associated with this attack?

### What IP address has P01s0n1vy tied to domains that are pre-staged to attack Wayne Enterprises?

### Based on the data gathered from this attack and common open source intelligence sources for domain names, what is the email address that is most likely associated with P01s0n1vy APT group?

### GCPD reported that common TTPs (Tactics, Techniques, Procedures) for the P01s0n1vy APT group if initial compromise fails is to send a spear phishing email with custom malware attached to their intended target. This malware is usually connected to P01s0n1vy’s initial attack infrastructure. Using research techniques, provide the SHA256 hash of this malware.

### What special hex code is associated with the customized malware discussed in the previous question?

### What does this hex code decode to?
