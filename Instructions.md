**Wiring:**

  

ADXL345 pin RPi Pico pin    RPi Pico pin name

VIN (or VCC)    36  3V3 Out

GND 38  GND

CS  2   SPI0 CSn (GP 1)

SDO 1   SPI0 RX (GP 0)

SDA 5   SPI0 TX (GP 3)

SCL 4   SPI0 SCK (GP 2)

  ![[Pasted image 20250115210413.png]]

---

  

**Compile the firmware:**

  

`cd ~/klipper/`

`make menuconfig`

Micro-Controller Architecture (Raspberry Pi RP2040)  --->
  ![[Pasted image 20250115210546.png]]


make

  

---

  

**Flash the firmware:**

  
1. Conncect the Pico to the regular Klipper Raspberry Pi (or other SBC) USB port, while holding down the BOOTSEL button.

2. Given no other mass storage devices are existing, the Pico should register as block device /dev/sda.

3. Mount the block device and copy the Klipper firmware file to it


`sudo mount /dev/sda1 /mnt`

`sudo cp out/klipper.uf2 /mnt`

`sudo umount /mnt`

***OR***

1. Use WinSCP to copy the firmware file onto your PC.

2. Hold down the bootsel button while plugging the pico into the PC.

---

# Configuring Klipper

1. Get the correct `serial` path with `ls /dev/serial/by-id/*`
2. Add the following to the `printer.cfg` file:

`[mcu pico]`
`serial: /dev/serial/by-id/usb-Klipper_rp2040_mySerial`

`[adxl345]`
`spi_bus: spi0a`
`cs_pin: pico:gpio1`

`[resonance_tester]`
`accel_chip: adxl345`
`probe_points:`
    `100,100,20 # an example`


3. Restart Klipper with the `RESTART` command.

---

1. create a file in klipper called picoadxl.cfg

2. [include] that file at the top of your printer.cfg

3. Do your resonance compensation stuff

4. when your done, comment out the [include] so that you don't get errors.