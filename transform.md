### Device setup
- Jumper settings
- Boot from SD card, turn into ADALM-PLUTO compatible mode or USRP B210 compatible mode
![How to boot](https://github.com/signalens/signalsdrpro/blob/main/img/transform/boot_ins.png?raw=true)

### Connector layout
![Overview](https://github.com/signalens/signalsdrpro/blob/main/img/transform/layout.png?raw=true)

#### USRP B210 compatible mode
- Download [signalsdrpro_b210.bin](https://github.com/signalens/signalsdrpro/tree/main/bin/b210) and save as BOOT.BIN
- Copy BOOT.BIN into SD card root folder
- Power/Console: USB Type C
- Data: USB type B

```
PCAP DMA DEST LEN 0xF8007024: 0x000F6EC0
PCAP ROM SHADOW CTRL 0xF8007028: 0xFFFFFFFF
PCAP MBOOT 0xF800702C: 0x0000C000
PCAP SW ID 0xF8007030: 0x00000000
PCAP UNLOCK 0xF8007034: 0x757BDF0D
PCAP MCTRL 0xF8007080: 0x38800100

DMA Done !

FPGA Done !
In FsblHookAfterBitstreamDload function
Partition Number: 2
Header Dump
Image Word Len: 0x00002002
Data Word Len: 0x00002002
Partition Word Len:0x00002002
Load Addr: 0x00100000
Exec Addr: 0x00100000
Partition Start: 0x000FE490
Partition Attr: 0x00000010
Partition Checksum Offset: 0x00000000
Section Count: 0x00000001
Checksum: 0xFFCFB8F8
Application
Handoff Address: 0x00100000
In FsblHookBeforeHandoff function
SUCCESSFUL_HANDOFF
FSBL Status = 0x1
SDRPRO B210 Hello World
SDRPRO B210 Hello World
```

#### ADALM-PLUTO compatible mode
- Download [all files](https://github.com/signalens/signalsdrpro/tree/main/bin/adalm-pluto/)
- copy all files into SD card root folder
- Power/Console: USB Type C
- Data: Micro USB port 2
- Optional: Also try [tezuka_fw](https://github.com/F5OEO/tezuka_fw) for more advanced feature
- Username: root Password: analog

```
signalsdrpro login: root
Password:
\033[1;35m
___________                  __
\__    ___/___ __________ __|  | _______
  |    |_/ __ \\___   /  |  \  |/ /\__  \
  |    |\  ___/ /    /|  |  /    <  / __ \_
  |____| \___  >_____ \____/|__|_ \(____  /
             \/      \/          \/     \/
Universal Zynq70x/AD936x firmware builder
Version tezuka-0.2.4
By F5OEO (2024-2025)
\033[0m

# uname -a
Linux signalsdrpro 6.1.0 #1 SMP PREEMPT Tue Jan 20 11:52:38 CST 2026 armv7l GNU/Linux
#
```

### Reset SPI (Only if stuck at u-boot)
- Option 1: Reset to default environment and save
```
env default -a
saveenv
```
- Option 2: Erase the environment section of flash (more aggressive)
```
sf probe
sf erase 0x100000 0x20000
```
