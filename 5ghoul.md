### Tested enviroment:
- Host OS: Windows 11
- Virtual Machine: VMware Workstation 17
- VM: Debian 12 / DragonOS
- Tested with Docker

### Phone Setup
- Android with 5G phone
- Goto "Developers options"
- Under "Networking" -> "5G Network Mode"
  - Choose "SA+NSA mode"
  - Turn on USB Debug

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
- Edit configs/global_config.json, look for UEModemManager
  - "EnableADB" change to true
  - "ADBDevice" change to the device it
- Goto configs/5gnr_gnb and run cp n78.106.conf b210.conf
- Edit configs/5gnr_gnb/open5gs.yaml and delete all the IPv6 related "- ::1"
  
Testing
```
sudo bin/5g_fuzzer --MCC=001 --MNC=01 --GlobalTimeout=false --EnableMutation=false
```

Fuzz
```
sudo bin/5g_fuzzer --MCC=001 --MNC=01 --GlobalTimeout=false --EnableMutation=true --BaseStationConfigFile b210.conf
```


-- Still WIP --
