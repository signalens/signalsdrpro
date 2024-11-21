Tested enviroment:
- Host OS: Debian Bookworm
- Docker

### Device setup
- Jumper settings
- Boot from SD card with and run as USRP B210/X310
![How to boot](https://github.com/signalens/signalsdrpro_docs/blob/main/img/boot_ins.png?raw=true)

### Docker env setup

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

### Insatll Dockers

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Insatll Open5GS

- Build docker images for open5gs EPC/5GC components
```
git clone https://github.com/herlesupreeth/docker_open5gs
cd docker_open5gs/base
sudo docker build --no-cache --force-rm -t docker_open5gs .
```
- Build docker images for kamailio IMS components
```
cd ../ims_base
sudo docker build --no-cache --force-rm -t docker_kamailio .
```
- Build docker images for srsRAN_4G eNB + srsUE (4G+5G)
```
cd ../srslte
sudo docker build --no-cache --force-rm -t docker_srslte .
```
- Build docker images for srsRAN_Project gNB
```
cd ../srsran
sudo docker build --no-cache --force-rm -t docker_srsran .
```
- Build docker images for UERANSIM (gNB + UE)
```
- cd ../ueransim
sudo docker build --no-cache --force-rm -t docker_ueransim .
```


### Compile 4G and 5G baseband
```
sudo docker compose -f 4g-volte-deploy.yaml build
sudo docker compose -f sa-deploy.yaml build
```

### Update .env

Make sure MCC and MNC value is correct

```
MCC=001
MNC=01
DOCKER_HOST_IP --> This is the IP address of the host running your docker setup
UE_IPV4_INTERNET --> Optional: Change this to your desired (Not conflicted) UE network ip range for internet APN
UE_IPV4_IMS --> Optional: Change this to your desired (Not conflicted) UE network ip range for ims APN
```

Activate .env
```
cd ..
set -a
source .env
sudo ufw disable
sudo sysctl -w net.ipv4.ip_forward=1
sudo cpupower frequency-set -g performance
```


### Choose 1: Running 4G or 5G network

4G baseband
- 4G Core Network + IMS + SMS over SGs
- srsRAN eNB using SDR (OTA)，比如B210、X310
```
sudo docker compose -f 4g-volte-deploy.yaml up
sudo docker compose -f srsenb.yaml up -d && sudo docker container attach srsenb
```
#### - or -

5G baseband
- 5G Core Network, Core
- srsRAN gNB using SDR (OTA), as B210/X310
```
sudo docker compose -f sa-deploy.yaml up
sudo docker compose -f srsgnb.yaml up -d && sudo docker container attach srsgnb
```

### Optional: Running 4G/5G emulator

4G baseband
``` 
# srsRAN ZMQ eNB (RF simulated)emulator
docker compose -f srsenb_zmq.yaml up -d && docker container attach srsenb_zmq
# srsRAN ZMQ 4G UE (RF simulated), Emulator
docker compose -f srsue_zmq.yaml up -d && docker container attach srsue_zmq
```

#### - or -

5G baseband
```
# srsRAN ZMQ gNB (RF simulated) Emulator
docker compose -f srsgnb_zmq.yaml up -d && docker container attach srsgnb_zmq
# srsRAN ZMQ 5G UE (RF simulated) Emulator
docker compose -f srsue_5g_zmq.yaml up -d && docker container attach srsue_5g_zmq
# UERANSIM gNB (RF simulated) Emulator
docker compose -f nr-gnb.yaml up -d && docker container attach nr_gnb
# UERANSIM NR-UE (RF simulated) Emulator
docker compose -f nr-ue.yaml up -d && docker container attach nr_ue
```

### Adding Sim Card

```
sudo docker exec -it hss misc/db/open5gs-dbctl add 001010000000001 \
00112233445566778899AABBCCDDEEFF 000102030405060708090A0B0C0D0E0F
sudo docker exec -it hss misc/db/open5gs-dbctl add 001010000000002 \
00112233445566778899AABBCCDDEEFF 000102030405060708090A0B0C0D0E0F
sudo docker exec -it hss misc/db/open5gs-dbctl add 001010000000003 \
00112233445566778899AABBCCDDEEFF 000102030405060708090A0B0C0D0E0F
```

### Startup Script
Create start up script
```startbaseband.sh
#!/bin/bash

set -a
source .env

sudo ufw disable
sudo sysctl -w net.ipv4.ip_forward=1
sudo cpupower frequency-set -g performance

if [ $1 != '4g' ];
    sudo docker compose -f 4g-volte-deploy.yaml up
    sudo docker compose -f srsenb.yaml up -d && sudo docker container attach srsenb
elif [ $1 != '5g' ];
    sudo docker compose -f sa-deploy.yaml up
    sudo docker compose -f srsgnb.yaml up -d && sudo docker container attach srsgnb
fi
```

Start 4G baseband
```
./startbaseband.sh 4g
```

#### - or -
Start 5G Network
```
./startbaseband.sh 5g
```
