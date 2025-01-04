### Enviroment:
- Host OS: Windows 11 / Linux
- Virtual Machine: VirtualBox or VMware Workstation 17
- [Disable Hyper-V if using Windows](https://github.com/xwings/tuya/blob/master/notes/turnoffwindows11hyperv.md)
- VM: Debian 12
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

![Phone baseband](https://github.com/signalens/signalsdrpro/blob/main/img/5ghoul/fuzzing.png?raw=true)
