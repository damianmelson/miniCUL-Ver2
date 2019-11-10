# miniCUL Version 2
RF-Gerät zum Empfangen und Senden verschiedener 433 und 868 MHz Funkprotokolle.

![Ansicht](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/ansicht_oben.jpg)
![Ansicht](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/ansicht_unten.jpg)

## Schaltplan
- 433 MHz

![Schaltplan](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/schaltplan_433.png)

- 868 MHz

![Schaltplan](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/schaltplan_868.png)

## Platine
[Gerberdaten](https://github.com/damianmelson/miniCUL-Ver2/tree/master/gerber) für [PCBWay](https://www.pcbway.com) und [ALLPCB](https://www.allpcb.com).

![Platine oben](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/pcb_oben.png)
![Platine unten](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/pcb_unten.png)

[Bauelemente](https://github.com/damianmelson/miniCUL-Ver2/tree/master/docs) von [reichelt elektronik](https://www.reichelt.de) und [LCSC](https://lcsc.com).<br>
U1 bestückbar mit 433/868 MHz CC1101-Funkmodul von [eBay](https://www.ebay.de).

## Bootloader
[Optiboot Bootloader](https://github.com/damianmelson/miniCUL-Ver2/tree/master/bootloader) für ATmega328P (3.3 Volt, 8 MHz, 57600 Baud).<br>
ATmega328P Fuses:

Fuse | Wert
---- | ----
LOW  | 0xFF
HIGH | 0xDA
EXT  | 0xFD
LOCK | 0xFF

Bootloader brennen:<br>
`avrdude -c usbtiny -p m328p -U flash:w:optiboot_atmega328p.hex:i -U lfuse:w:0xFF:m -U hfuse:w:0xDA:m -U efuse:w:0xFD:m -U lock:w:0xFF:m`<br>

Option -c ist anzupassen an den aktuell benutzten ISP-Programmer.

## Firmware
[a-culfw](https://github.com/heliflieger/a-culfw/tree/master/culfw/Devices/miniCUL) für miniCUL 433 MHz flashen:<br>
`avrdude -p atmega328p -c arduino -P /dev/ttyUSBx -b 57600 -U flash:w:miniCUL_433MHZ.hex:i`<br>
[a-culfw](https://github.com/heliflieger/a-culfw/tree/master/culfw/Devices/miniCUL) für miniCUL 868 MHz flashen:<br>
`avrdude -p atmega328p -c arduino -P /dev/ttyUSBx -b 57600 -U flash:w:miniCUL_868MHZ.hex:i`<br>

USBx ist anzupassen an die aktuell benutzte Schnittstelle.

## esp-link
[esp-link](https://github.com/jeelabs/esp-link) flashen:<br>
- GPIO0 am PSF-A85 mit GND verbinden.<br>
- 3V3, TX, RX und GND am PSF-A85 mit einem USB zu TTL Adapter (3.3V Logikpegel) verbinden.<br>
- [Transparent Bridge Firmware](https://github.com/jeelabs/esp-link/releases) flashen:<br>
`esptool.py --port /dev/ttyUSBx --baud 230400 write_flash --flash_size 1MB --flash_mode dout --flash_freq 40m 0x00000 boot_v1.6.bin 0x01000 user1.bin 0xFC000 esp_init_data_default.bin 0xFE000 blank.bin`<br>
USBx ist anzupassen an die aktuell benutzte Schnittstelle.<br>
- Verbindungen entfernen.<br>

[esp-link](https://github.com/jeelabs/esp-link) Konfiguration für miniCUL:<br>
- Home, Pin assignment

![Pin assignment](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/pin_assignment.png)

- µC Console

![µC Console](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/µc_console.png)

- Debug log

![Debug log](https://github.com/damianmelson/miniCUL-Ver2/blob/master/images/debug_log.png)

## FHEM
miniCUL in [FHEM](https://wiki.fhem.de/wiki/CUL) anlegen:<br>
`define miniCUL CUL /dev/ttyUSBx@38400 1234`<br>

USBx ist anzupassen an die aktuell benutzte Schnittstelle.<br>

miniCUL im WLAN-Modus anlegen:<br>
`define miniCUL CUL <IP>:23 1234`<br>

IP ist anzupassen an die aktuell benutzte IP-Adresse.