# [Wonderland](https://tryhackme.com/room/wonderland)
## Summary:
- Gobuster through many directories until we find creds in the page source
- Create a shell in random.py to get a shell for rabbit when the walrus file runs

6/22/22


![image](https://user-images.githubusercontent.com/66894542/175207374-0e187458-d43e-440d-b5a8-8c60fe15219c.png)

- Nothing special except for the web server so lets go there and we will find a page with a picure and some words, so lets try directory brute forcing!

![image](https://user-images.githubusercontent.com/66894542/175207817-d5caaf45-9b87-4b50-b107-f3886fe33eff.png)

- Looks like we need to do some recursion, but gobuster does not offer recursion, so I assumed after directories /r/a the rest would spell out to rabbit:

![image](https://user-images.githubusercontent.com/66894542/175208355-e951e545-959e-4b80-afd9-6b30885a3675.png)

- There was not much on the last page so I viewed the source and there was some text that is not displayed which could be credentials?:

![image](https://user-images.githubusercontent.com/66894542/175208877-1022094b-6e46-4462-9942-ab2af5e442ba.png)

- I sshed in and it worked! Then I found the root flag but don't have permissions:

![image](https://user-images.githubusercontent.com/66894542/175209394-44a42f96-1f5a-4da0-9a3c-10f391f1db2f.png)

- The hint says everything is upside down so I checked in the /root directory and it was there!

- We also can run the walrus file with sudo with rabbit:

![image](https://user-images.githubusercontent.com/66894542/175209953-d2fa343c-ac66-4688-93f5-f35d30282254.png)

- The walrus file is interesting and all it does is display 10 random lines out of a poem. Looking at the contents, it imports the random package which we can make:

![image](https://user-images.githubusercontent.com/66894542/175457921-a12a44d6-b717-4a7d-b532-18a27a02b5ec.png)

- In [GTFOBins](https://gtfobins.github.io/gtfobins/python/#sudo), we can escalate our privileges if we can sudo with python so lets use that!

- We can create a file named random.py with just `import os; os.system("/bin/sh")` since we are already running sudo python.
- Now we just need to run the command: `sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py`

- We got a shell as rabbit! Looking at rabbit's home directory, there is a file named teaParty:
![image](https://user-images.githubusercontent.com/66894542/175459196-a6cff2a0-167a-463e-a59f-53d14328461c.png)
![image](https://user-images.githubusercontent.com/66894542/175459384-1141f0d6-90b4-4df8-8d27-0c0915ff1972.png)

