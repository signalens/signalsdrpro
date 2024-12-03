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

Run and fuzz
```
./container.sh run release-5g # This will pull and start the terminal of the 5Ghoul container
sudo bin/5g_fuzzer --MCC=001 --MNC=01 --GlobalTimeout=false --EnableMutation=false # Start the base station inside the container
```


-- Still WIP --
