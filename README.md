#Bootloader for Bigtreetech SKR 1.3, 1.4 and 1.4 TURBO

This is a fork of https://github.com/triffid/LPC17xx-DFU-Bootloade.

Supports Bigtreetech SKR 1.3, SKR 1.4 and SKR 1.4 TURBO with the same binary.

##SD card support
By default the SD card (SPI 1) on the SKR as well as the SD card on a connected display board (SPI 0) (if connected) may be used for updating the firmware.

As in the original code a file named "firmware.bin" on the FAT32-formatted SD card's first partition, will be flashed to the application region (0x4000 after the 16k space for the bootloader), and after successful flashing, the file will be renameed to "firmware.cur" - 'cur' meaning 'current'.

##USB support

In addition, Flash via USB is enabled as default. It will be activated:
- if no firmware is installed (Flash @16K = 0xffffffff)
- if the button on a connected display board (e.g. BTT LCD-35 or RepRapDiscount Full Graphic Smart Controller) is pressed while booting. (pin 2 of EXP1 = 0.28 connected to GND).

Firmware can than be uploaded via dfu-tool, e.g. `dfu-tool write firmware.bin`

##Silent beeper

EXP1 pin 1 (1.30) will be configured as output and set to 0 to avoid endless beeping of the beeper on a connected RepRapDiscount Full Graphic Smart Controller.

##Installing the bootloader

I tried it with a serial connection first but somehow that did not worked for me. I got the "Synced.." message after sending ? but most other characters send were echo'ed differently. Seens to be some baudrate problem, gave up after several hours of probing.

Using a ST-Link V2 clone with openocd worked fine.

Install openocd and connect the ST-Link with 3 wires (btt powered via USB or 12/24V) as follows:

`ST-Link     BTT`
`----------------------------`
`SWDIO       SWDIO (J2 Pin 2)`
`SWCLK       SWDCLK (J2 Pin 4)`
`GND         GND (J2 Pin 3) or some other GND pin, e.g. middle pin of one endswitch connector`

and run `make upload` or start the `writebootloader` script in bttskr/.
You can backup your current bootboader or the whole flash using the scripts `readbootloader` or `readflash`.

##Configuration
By default, the bootloader will show status/debug messages via the serial port @115200 baud. You can configure these as well as disableing USB, disabling the second SD card and so on in `config.h`.

##Why
I managed to destroy the SD card slot on my 1.4 board and while trying to solder a new one, i destroyed some pads on the board. Insead of using another board i now use the SD card slot on the display board for firmware uploads.
