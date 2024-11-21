Tested enviroment:
- Host OS: Any OS with MATLAB installed

### Device setup
- Jumper settings
- Boot from SD card, turn into PlutoSDR or USRP
![How to boot](https://github.com/signalens/signalsdrpro_docs/blob/main/img/boot_ins.png?raw=true)

### SD Card firmware
- Download
```
wget https://github.com/signalens/firmware/raw/refs/heads/main/signalsdrpro/signalsdrpro_b210.bin -o BOOT.BIN
```
-  Copy BOOT.BIN into SD card root folder

#### - or -

- Download https://github.com/signalens/signalsdr-fw-bin/tree/main/pro-2023.2
- copy all the files into SD card root folder

### DragonOS

- Boot into DragonOS
- Start GNU Radio as USRP
- Run as B210 or PlutoSDR
