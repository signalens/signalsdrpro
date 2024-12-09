### Enviroment:
- Host OS: Windows 11 / Linux
- Virtual Machine: VirtualBox or VMware Workstation 17
- Disable Hyper-V if using Windows
- VM: Debian 12
- Docker

### Phone Setup
- Android with 5G phone
- Goto "Developers options"
- Under "Networking" -> "5G Network Mode"
  - Choose "SA+NSA mode"

### Steps to disbale Hyper-V

[Here](https://github.com/xwings/tuya/blob/master/notes/turnoffwindows11hyperv.md)

### Turn SignalSDR Pro into USRP B210 compatible mode
- Turn SignalSDR Pro into USDP B210 compatible mode [How](https://github.com/signalens/signalsdrpro_docs/blob/main/transform.md)

### Docker env setup
Add Docker's official GPG key:
```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
```

#### Debian
```
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

#### Ubuntu
```
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

#### Install Docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin git vim
```

### Insatll Open5GS

Build docker images for open5gs EPC/5GC components
```
git clone https://github.com/herlesupreeth/docker_open5gs
cd docker_open5gs/base
sudo docker build --no-cache --force-rm -t docker_open5gs .
```

Build docker images for kamailio IMS components
```
cd ../ims_base
sudo docker build --no-cache --force-rm -t docker_kamailio .
```

Build docker images for srsRAN_4G eNB + srsUE (4G+5G)
```
cd ../srslte
sudo docker build --no-cache --force-rm -t docker_srslte .
```

Build docker images for srsRAN_Project gNB
```
cd ../srsran
sudo docker build --no-cache --force-rm -t docker_srsran .
```

Build docker images for UERANSIM (gNB + UE)
```
cd ../ueransim
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


### Running 4G or 5G network

#### 4G baseband
- 4G Core Network + IMS + SMS over SGs
- srsRAN eNB using SDR (OTA)，USRP B210/X310 compatible mode
```
sudo docker compose -f 4g-volte-deploy.yaml up
sudo docker compose -f srsenb.yaml up -d && sudo docker container attach srsenb
```

#### 5G baseband
- 5G Core Network, Core
- srsRAN gNB using SDR (OTA), as B210/X310
```
sudo docker compose -f sa-deploy.yaml up
sudo docker compose -f srsgnb.yaml up -d && sudo docker container attach srsgnb
```

### Additional steps needed if using Virtual Machine

Need to make few more tries if these error appears. Make sure attatch "WestBridge" follow by "USRP B200". One at a time

![Load usrp_b200_fw.hex](https://github.com/signalens/signalsdrpro_docs/blob/main/img/open5gs/load_firmware.png?raw=true)

### Note for WSL2

Install usbipd (Windows PowerShell)
```
winget install --interactive --exact dorssel.usbipd-win
```

List USB devices (Windows PowerShell)
```
usbipd list
```

Bind and attach USB devices (Windows PowerShell)
```
usbipd bind --busid 4-4
usbipd attach --wsl --busid <busid>
```

Detach USB devices (Windows PowerShell)
```
usbipd detach --busid <busid>
```

![Bind both USB](https://github.com/signalens/signalsdrpro_docs/blob/main/img/open5gs/wsl_bind_usb.png?raw=true)

### Successful execution

![Open5gs run](https://github.com/signalens/signalsdrpro_docs/blob/main/img/open5gs/open5gs_good_exec.png?raw=true)

![Phone baseband](https://github.com/signalens/signalsdrpro_docs/blob/main/img/open5gs/phone_baseband.jpg?raw=true)


### Provisioning of SIM information in open5gs HSS as follows:
Open (http://<DOCKER_HOST_IP>:9999) in a web browser, where <DOCKER_HOST_IP> is the IP of the machine/VM running the open5gs containers. Login with following credentials

Username : admin
Password : 1423

```
IMSI: 001010000000001
Subscriber Key (K): 00112233445566778899AABBCCDDEEFF
Operator Key (OPc/OP): 000102030405060708090A0B0C0D0E0F

IMSI: 001010000000002 
Subscriber Key (K): 00112233445566778899AABBCCDDEEFF
Operator Key (OPc/OP): 000102030405060708090A0B0C0D0E0F

IMSI: 001010000000003
Subscriber Key (K): 00112233445566778899AABBCCDDEEFF
Operator Key (OPc/OP): 000102030405060708090A0B0C0D0E0F
```

### Preparing simcard
You need a "blank" simcard and simcard writer to write into the simcard

### Prepare Startup Script

To make life simple create startup script

```startbaseband.sh
#!/bin/bash

set -a
source .env

sudo ufw disable
sudo sysctl -w net.ipv4.ip_forward=1
sudo cpupower frequency-set -g performance

if [ "$1" == '4g' ]; then
  tmux new-session -d -s startbaseband "docker compose -f 4g-volte-deploy.yaml up"
  tmux split-window -t startbaseband:0 -v -p 20 "docker compose -f srsenb.yaml up -d && sudo docker container attach srsenb"
elif [ "$1" == '5g' ]; then
  tmux new-session -d -s startbaseband "docker compose -f sa-deploy.yaml up"
  tmux split-window -t startbaseband:0 -v -p 20 "docker compose -f srsgnb.yaml up -d && sudo docker container attach srsgnb"
else
  echo "Choose either 4g or 5g"
fi
```

### Optional: 

#### Running 4G/5G emulator

4G baseband
``` 
# srsRAN ZMQ eNB (RF simulated)emulator
docker compose -f srsenb_zmq.yaml up -d && docker container attach srsenb_zmq
# srsRAN ZMQ 4G UE (RF simulated), Emulator
docker compose -f srsue_zmq.yaml up -d && docker container attach srsue_zmq
```

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
