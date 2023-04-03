
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
sudo apt install docker.io -y
```

### Download the Script
```
wget https://raw.githubusercontent.com/theNetworkChuck/NetworkChuck/master/pihole.sh
```

### Or Manually copy from below with latest version

Create a file with `nano pihole.sh` and copy the below code
```
#!/bin/bash

# https://github.com/pi-hole/docker-pi-hole/blob/master/README.md

docker run -d \
    --name pihole \
    -p 53:53/tcp -p 53:53/udp \
    -p 80:80 \
    -p 443:443 \
    -p 8080:8080 \
    -e TZ="Asia/Kolkata" \
    -e ServerIP="YOUR_PUBLIC_IP_ADDRESS" \
    -e WEBPASSWORD="CHANGE-ME" \
    -v "$(pwd)/etc-pihole/:/etc/pihole/" \
    -v "$(pwd)/etc-dnsmasq.d/:/etc/dnsmasq.d/" \
    --dns=127.0.0.1 --dns=1.1.1.1 \
    --restart=unless-stopped \
    pihole/pihole:latest

printf 'Starting up pihole container '
for i in $(seq 1 20); do
    if [ "$(docker inspect -f "{{.State.Health.Status}}" pihole)" == "healthy" ] ; then
        printf ' OK'
        echo -e "\n$(docker logs pihole 2> /dev/null | grep 'password:') for your pi-hole: https://${IP}/admin/"
        exit 0
    else
        sleep 3
        printf '.'
    fi

    if [ $i -eq 20 ] ; then
        echo -e "\nTimed out waiting for Pi-hole start, consult check your container logs for more info (\`docker logs pihole\`)"
        exit 1
    fi
done;
© 2020 GitHub, Inc.
```
 - Change `CHANGE-ME`, `YOUR_PUBLIC_IP_ADDRESS` and `TZ`
 - Change the Script Permission with `chmod u+x pihole.sh`

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
 

## Troubleshooting

### To emove Docker Container and Image follow below command
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
