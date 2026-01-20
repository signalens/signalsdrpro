### Device setup
- Jumper settings
- Boot from SD card, turn into ADALM-PLUTO compatible mode or USRP B210 compatible mode
![How to boot](https://github.com/signalens/signalsdrpro/blob/main/img/transform/boot_ins.png?raw=true)

### Connector layout
![How to boot](https://github.com/signalens/signalsdrpro/blob/main/img/transform/layout.png?raw=true)

#### USRP B210 compatible mode
- Download [signalsdrpro_b210.bin](https://github.com/signalens/signalsdrpro/tree/main/bin/b210) and save as BOOT.BIN
- Copy BOOT.BIN into SD card root folder
- Power/Console: USB Type C
- Data: USB type B

#### ADALM-PLUTO compatible mode
- Download [all files](https://github.com/signalens/signalsdrpro/tree/main/bin/adalm-pluto/)
- copy all files into SD card root folder
- Power/Console: USB Type C
- Data: Micro USB port 2
- Optional: Also try [tezuka_fw](https://github.com/F5OEO/tezuka_fw) for more advanced feature
