SignalSDR Pro can switch into different software-defined radio (SDR) modes, including USRP B210 and ADALM-PLUTO. This is possible because the device boots from an SD card and loads the appropriate FPGA bitstream, firmware, and configuration files for the selected mode. The sections below explain how to prepare the SD card and connect the device for each mode.

### Device setup
- Jumper settings
- Boot from SD card, switch to ADALM-PLUTO compatible mode or USRP B210 compatible mode
![How to boot](https://github.com/signalens/signalsdrpro/blob/main/img/transform/boot_ins.png?raw=true)

### Connector layout
![Overview](https://github.com/signalens/signalsdrpro/blob/main/img/transform/layout.png?raw=true)

#### USRP B210 compatible mode
- Download [signalsdrpro_b210.bin](https://github.com/signalens/signalsdrpro/tree/main/bin/b210) and save as BOOT.BIN
- Copy BOOT.BIN into SD card root folder
- Power/Console: USB Type-C (Must be type A to C)
- Data: USB Type-B
- Additional notes:
  - Always complete power cycle before running next round of test
  - Pull of type C and type B port, wait for 2 seconds and plug in again
  - It should detect as "Product: WestBridge" and NOT "Product: USRP B200"
  - Only after the process runs correctly, it will turn into "Product: USRP B200"
```
[500540.671574] usb 3-3: New USB device found, idVendor=2500, idProduct=0020, bcdDevice= 1.00
[500540.671582] usb 3-3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[500540.671583] usb 3-3: Product: WestBridge
[500540.671585] usb 3-3: Manufacturer: Cypress
```
```
[499983.760504] usb 4-3: New USB device found, idVendor=2500, idProduct=0020, bcdDevice= 0.00
[499983.760507] usb 4-3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[499983.760509] usb 4-3: Product: USRP B200
[499983.760510] usb 4-3: Manufacturer: Ettus Research LLC
```
  
![LED](https://github.com/signalens/signalsdrpro/blob/main/img/transform/b210.png?raw=true)

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
```
xwings@hoth:~$ UHD_IMAGES_DIR=/usr/share/uhd/4.8.0/images uhd_usrp_probe
[INFO] [UHD] linux; GNU C++ version 14.2.0; Boost_108300; UHD_4.8.0.0+ds1-2
[INFO] [B200] Loading firmware image: /usr/share/uhd/4.8.0/images/usrp_b200_fw.hex...
[INFO] [B200] Detected Device: B210
[INFO] [B200] Loading FPGA image: /usr/share/uhd/4.8.0/images/usrp_b210_fpga.bin...
[INFO] [B200] Operating over USB 3.
[INFO] [B200] Detecting internal GPSDO....
[INFO] [GPS] Found an internal GPSDO: GPSTCXO v3.2 for SDRPro
[INFO] [B200] Initialize CODEC control...
[INFO] [B200] Initialize Radio control...
[INFO] [B200] Performing register loopback test...
[INFO] [B200] Register loopback test passed
[INFO] [B200] Performing register loopback test...
[INFO] [B200] Register loopback test passed
[INFO] [B200] Setting master clock rate selection to 'automatic'.
[INFO] [B200] Asking for clock rate 16.000000 MHz...
[INFO] [B200] Actually got clock rate 16.000000 MHz.
  _____________________________________________________
 /
|       Device: B-Series Device
|     _____________________________________________________
|    /
|   |       Mboard: B210
|   |   serial: 194170
|   |   name: MyB210
|   |   product: 2
|   |   revision: 4
|   |   FW Version: 8.0
|   |   FPGA Version: 16.0
|   |
|   |   Time sources:  none, internal, external, gpsdo
|   |   Clock sources: internal, external, gpsdo
|   |   Sensors: gps_gpgga, gps_gprmc, gps_time, gps_locked, gps_servo, ref_locked
|   |     _____________________________________________________
|   |    /
|   |   |       RX DSP: 0
|   |   |
|   |   |   Freq range: -8.000 to 8.000 MHz
|   |     _____________________________________________________
|   |    /
|   |   |       RX DSP: 1
|   |   |
|   |   |   Freq range: -8.000 to 8.000 MHz
|   |     _____________________________________________________
|   |    /
|   |   |       RX Dboard: A
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       RX Frontend: A
|   |   |   |   Name: FE-RX2
|   |   |   |   Antennas: TX/RX, RX2
|   |   |   |   Sensors: temp, rssi, lo_locked
|   |   |   |   Freq range: 50.000 to 6000.000 MHz
|   |   |   |   Gain range PGA: 0.0 to 76.0 step 1.0 dB
|   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
|   |   |   |   Connection Type: IQ
|   |   |   |   Uses LO offset: No
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       RX Frontend: B
|   |   |   |   Name: FE-RX1
|   |   |   |   Antennas: TX/RX, RX2
|   |   |   |   Sensors: temp, rssi, lo_locked
|   |   |   |   Freq range: 50.000 to 6000.000 MHz
|   |   |   |   Gain range PGA: 0.0 to 76.0 step 1.0 dB
|   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
|   |   |   |   Connection Type: IQ
|   |   |   |   Uses LO offset: No
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       RX Codec: A
|   |   |   |   Name: B210 RX dual ADC
|   |   |   |   Gain Elements: None
|   |     _____________________________________________________
|   |    /
|   |   |       TX DSP: 0
|   |   |
|   |   |   Freq range: -8.000 to 8.000 MHz
|   |     _____________________________________________________
|   |    /
|   |   |       TX DSP: 1
|   |   |
|   |   |   Freq range: -8.000 to 8.000 MHz
|   |     _____________________________________________________
|   |    /
|   |   |       TX Dboard: A
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       TX Frontend: A
|   |   |   |   Name: FE-TX2
|   |   |   |   Antennas: TX/RX
|   |   |   |   Sensors: temp, lo_locked
|   |   |   |   Freq range: 50.000 to 6000.000 MHz
|   |   |   |   Gain range PGA: 0.0 to 89.8 step 0.2 dB
|   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
|   |   |   |   Connection Type: IQ
|   |   |   |   Uses LO offset: No
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       TX Frontend: B
|   |   |   |   Name: FE-TX1
|   |   |   |   Antennas: TX/RX
|   |   |   |   Sensors: temp, lo_locked
|   |   |   |   Freq range: 50.000 to 6000.000 MHz
|   |   |   |   Gain range PGA: 0.0 to 89.8 step 0.2 dB
|   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
|   |   |   |   Connection Type: IQ
|   |   |   |   Uses LO offset: No
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       TX Codec: A
|   |   |   |   Name: B210 TX dual DAC
|   |   |   |   Gain Elements: None
```

#### ADALM-PLUTO compatible mode
- Download [all files](https://github.com/signalens/signalsdrpro/tree/main/bin/adalm-pluto/)
- Copy all files into SD card root folder
- Power/Console: USB Type-C (Must be type A to C)
- Data: Micro USB port 1
- Receiver: RX1
- Optional: Try [tezuka_fw](https://github.com/F5OEO/tezuka_fw) for more advanced features
- Username: root; Password: analog

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

### Reset SPI (Only if stuck at U-Boot)
- Connect to the Type-C port
```
screen /dev/ttyUSB1 115200
```
- In U-Boot mode, if SIGNALSDR> appears
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
