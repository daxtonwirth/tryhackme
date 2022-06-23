# [Wonderland](https://tryhackme.com/room/wonderland)
## Summary:
- Gobuster through many directories until we find creds in the page source
- Do something with the walrus file

10.10.66.77
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

- The walrus file is interesting and all it does is display 10 random lines out of a poem. We also can run the file with sudo. Looks like we need to do something with the user rabbit:

![image](https://user-images.githubusercontent.com/66894542/175209953-d2fa343c-ac66-4688-93f5-f35d30282254.png)
- I could not find anything else so I will come back to this later
