
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

 - Chnage TZ as per your region
 - Change the Script Permission with `chmod u+x pihole.sh`

### Run the Script
```
./pihole.sh
```
 - Open pihole on `http://SERVER-IP/admin`

### Change the Default pihole password
```
sudo docker exec -it pihole bash
sudo -a -p
```

 - The Big Blocklist Collection 1 [firebog.net](https://firebog.net/)
 - The Big Blocklist Collection 2 [github.developerdan.com/hosts](https://www.github.developerdan.com/hosts/)
 

### To emove Docker Container and Image follow below command
```
docker stop CONTAINER-NAME
docker rm CONTAINER-NAME
docker rmi DOCKER -IMAGE
```

