### Tested Enviroment:
- Host OS: Debian 12
- Docker

### TL;DR
[![5G Fuzzing](https://img.youtube.com/vi/XXR_ih3aO1g/0.jpg)](https://www.youtube.com/watch?v=XXR_ih3aO1g)

### Phone Setup
- Android with 5G phone
  - 5G must supprt band N78
- Disable "Smart 5G"
- Goto "Developers options"
  - Under "Networking" -> "5G Network Mode"
    - Choose "SA+NSA mode"
  - Turn on USB Debug
  - Disable "Veify apps over USB"
  - Disable "Verify bytecode of debuggable apps"
  - Disable "Verify bytecode of debuggable apps"
  - Enable "Disable child process restrictions"
  - Enbale "Disable permission monitoring"

### Turn SignalSDR Pro into USRP B210 compatible mode
- Turn SignalSDR Pro into USDP B210 compatible mode [How](https://github.com/signalens/signalsdrpro_docs/blob/main/transform.md)

### Download 5Ghoul
Download the installation script
```
mkdir 5ghoul
cd 5ghoul
curl -LJO https://github.com/asset-group/5ghoul-5g-nr-attacks/raw/master/container.sh
chmod +x container.sh
```

Install
```
./container.sh run release-5g
```

Setup
- run 3rd-party/adb/adb devices to check device id
- Edit configs/global_config.json look for UEModemManager
  - "EnableADB" change to true
  - "ADBDevice" change to the device ID
- Goto configs/5gnr_gnb_config.json
  - "ADBDevice" leave empty
- Goto configs/5gnr_gnb and run cp n78.106.conf signalsdrpro.conf
- Edit configs/5gnr_gnb/open5gs.yaml and delete all the IPv6 related "- ::1"
- Simcard settings (both simcard and in configs/5gnr_gnb_config.json)
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

Run after intial setup / install
```
docker ps -a | grep 5ghoul
docker start <DDOCKER_ID>
docker exec -it <DDOCKER_ID> /bin/bash
```

Testing
```
sudo bin/5g_fuzzer --MCC=001 --MNC=01 --GlobalTimeout=false --EnableMutation=false
```

Fuzz
```
sudo bin/5g_fuzzer --MCC=001 --MNC=01 --GlobalTimeout=false --EnableMutation=true --BaseStationConfigFile signalsdrpro.conf
```

Debug, check for USB latency
```
sudo 3rd-party/oai_5g_sa/cmake_targets/ran_build/build/nr-softmodem --sa --continuous-tx -E -O configs/5gnr_gnb/signalsdrpro.conf
```

Additional notes:
- Always complete power cycle before running next round of test
- Pull of type C and type B port, wait for 2 seconds and plug in again
- It should detect as "Product: WestBridge" and not "Product: USRP B200"
- Only after the process runs correctly, it will turn into "Product: USRP B200"
```
[500540.534897] usb 3-3: new high-speed USB device number 37 using xhci_hcd
[500540.671574] usb 3-3: New USB device found, idVendor=2500, idProduct=0020, bcdDevice= 1.00
[500540.671582] usb 3-3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[500540.671583] usb 3-3: Product: WestBridge
[500540.671585] usb 3-3: Manufacturer: Cypress
[500540.671585] usb 3-3: SerialNumber: 0000000004BE
```

![Phone baseband](https://github.com/signalens/signalsdrpro/blob/main/img/5ghoul/fuzzing.png?raw=true)
