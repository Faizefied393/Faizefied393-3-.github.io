# Faizefied393-3-.github.io
Cloud Wireguard VPN Documentation-Faiz Tariq
# Cloud Wireguard VPN Documentation-Faiz Tariq

# Using Docker Wireguard Cloud VPN

## The Use of Digital Ocean

1. This is th given link for use [link](https://m.do.co/c/4d7f4ff9cfe4) to build this Digital Ocean account with credit up to $200. 
2. When creating an account add a payment method. 
3. Go to the control panel and click **Droplets** in the left panel. 
4. You will want to create a Droplet that is new. First  click the **Marketplace** and then choose Docker on Ubuntu image. Next, change the CPU option to regular with SSD and select the $6/mo option. Set a root password and build this Droplet. 

## Wireguard Setup
1. Build a directory of Wireguard and its config files using these commands:
```Bash
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
```
2. Build this **.yml** file so you can copy paste the following contents for this into the file. Please change the **TZ** and **SERVERURL** appropriately. Also, grab the server url from the droplet's **Networking** tab under the **Public IPV4 Address**. 

```Bash
vim ~/wireguard/docker-compose.yml

version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - SERVERURL=165.232.143.113
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```

3. Run docker using this command: 
```Bash
cd ~/wireguard/
sudo docker-compose up -d
```

## Connecting the Phone

1. Use this command to generate this QR code in order to connect to your phone.
```Bash
docker logs wireguard
```
2. Scan the QR code with the mobile **Wireguard** app to create the VPN. 

### The Phone is Active VPN tunnel

![Imgur](https://imgur.com/UtigYAU.png)

### IP Address of Phone Before Wireguard is Enabled

![Imgur](https://i.imgur.com/LgTnZnE.png)

### The Phone IP Address After Wireguard is Enabled

![Imgur](https://imgur.com/MAU3Ocw.png)

## The PC is being Connected to

1. Changing this directory to **peer_pc1** in the this config file.
```Bash
cd ~/wireguard/config/peer_pc1/
```
2. Now printing the contents of the **peer_pc1.conf** file. Copying this text by having it printed out and creating a new document to paste the items into this document. Then change up this file extention from a **.txt** to a **.conf**.
3. Open this **Wireguard** application which is installed on your computer and then import the document. Finally, you can activate this VPN successfully. 

### The VPN Tunnel Active for Laptop
![Imgur](https://imgur.com/XRHqJk7.png)
### The IP Address of PC Before the Wireguard is Enabled
![Imgur](https://imgur.com/ql6v9q0.png)
### The on PC IP Address After Wireguard is Enabled
![Imgur](https://imgur.com/tlvEl9R.png)
