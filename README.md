
### Login as root
```
sudo su -
```

### Update System
```
sudo apt update && sudo apt upgrade -y
```

### Disbale Default DNS of Ubuntu
```
sudo systemctl stop systemd-resolved.service
sudo systemctl disable systemd-resolved.service
```

### Temporarily change DNS of Ubuntu Server
```
sudo nano /etc/resolv.conf
```
change `nameserver 8.8.8.8`

### Install Docker
```
wget https://raw.githubusercontent.com/dhruvdalsaniya21/misc-commands/main/docker-install.sh
```
change permission of script with `sudo chmod +x docker-install.sh`

### Download the Script
```
wget https://raw.githubusercontent.com/dhruvdalsaniya21/misc-commands/main/pihole.sh
```
 - Change the Script Permission with `sudo chmod u+x pihole.sh`
 - Change `CHANGE-ME`, `YOUR_PUBLIC_IP_ADDRESS` and `TZ`

### Run the Script
```
./pihole.sh
```
 - Open pihole on `http://SERVER-IP/admin`

### Change the Default pihole password
```
sudo docker exec -it pihole bash
sudo pihole -a -p
```

 - Test Pihole Ads Blocker
   - [d3ward.github.io/toolz/adblock](https://d3ward.github.io/toolz/adblock.html)
   - [fuzzthepiguy.tech/adtest](https://fuzzthepiguy.tech/adtest/)
   - [thepcspy.com/blockadblock/](https://thepcspy.com/blockadblock/)
 - Blocklist Collection [firebog.net](https://firebog.net/) and [github.developerdan.com/hosts](https://www.github.developerdan.com/hosts/)
 
#
### To remove Docker Container and Image follow below command (Troubleshooting)
```
docker stop CONTAINER-NAME
docker rm CONTAINER-NAME
docker rmi DOCKER -IMAGE
```
### Check the Pihole DNS Server IP
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' pihole
```
 - The output of the command should have same ip as your server's Public IP.

### restart stoped docker container
```
docker restart $(docker ps -a -q)
```
