### Enviroment:
- Host OS: Any OS

### Steps
- Download qflash.bin from https://github.com/signalens/signalsdrpro/tree/main/bin
- Copy both BOOT.BIN and qflash.bin to the root directory of the SD card.
- Insert the SD card into the board, set the boot mode to SD card mode.
- Connect the power supply, and wait for LD2 to blink, indicating that programming is complete.
- Set the boot mode to flash mode and power cycle the board.

The qflash.bin file provided in this folder is the boot file for SignalSDRPro in B210 mode. This qflash.bin file can also be renamed to boot.bin and copied to the SD card to run as a boot file in B210 mode.
