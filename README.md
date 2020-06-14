# tada68-bootloader-restore
This is the fix that I used to repair a bricked Tada68 keyboard

# Using a USBasp
Download the files, connect the pins of your ISP to the keyboard, I used a USBasp clone so you can change the commands as you need. The first step is connect the ISP. ![Tada68](https://rileywilbur.com/projects/tada68.jpg "tada68") 

The next step will be to flash the hex file. To flash th Atmel chip with the ISP using a USBasp clone use this command:

```bash
avrdude -c usbasp-clone -p atmega32u4 -F -e -U flash:w:mass_bootloader_tada68.hex
```
Then disconnect the ISP and connect the keyboard via USB. Then put it in to mass storage mode and drag the .BIN file to the keyboard and press ESC. DO NOT PRESS EJECT ON YOUR COMPUTER. If you press eject you will need to reflash the keyboard again using the ISP.


# Using a Raspberry Pi
## Sources
1. [Installing or unbricking a custom TADA68 bootloader using a Raspberry Pi](https://web.archive.org/web/20170714075038/http://maartendekkers.com/tada68/) by Maarten Dekkers
2. [How to un-brick Tada68 with Raspberry Pi](https://www.reddit.com/r/MechanicalKeyboards/comments/fu7rc0/how_to_unbrick_tada68_with_raspberry_pi/) by [/u/IamFonzy](https://www.reddit.com/user/IamFonzy/)

## Pre-work
1. Use Raspbian.  Ubuntu's GPIO setup wasn't right in ways I did not understand and did not want to troubleshoot.
2. install avrdude:
```
$ sudo apt-get install avrdude
```
3. clone this repo
```
$ git clone https://github.com/dlgoodr/tada68-bootloader-restore.git
```
4. create the avrdude config
```
$ cp /etc/avrdude.conf tada68-bootloader-restore/avrdude_gpio.conf
$ cat << EOF | tee -a tada68-bootloader-restore/avrdude_gpio.conf
programmer
  id    = "pi_1";
  desc  = "Use the Linux sysfs interface to bitbang GPIO lines";
  type  = "linuxgpio";
  reset = 8;
  sck   = 11;
  mosi  = 10;
  miso  = 9;
;
EOF
```

## Wiring up the Raspberry Pi
1. Disassemble the keyboard.  Pull the tab, g, h, \, ←, ↓, space bar, control and windows keycaps and remove the screws underneath.  Remove the board from it's case and flip it over, keys down.
2. Connect the rpi GPIO pins to their corresponding thruholes on the tada68.  I used an old PATA hard drive cable I had laying around, but whatever works.

![tada68](images/tada68.jpg?raw=true)

![rpi](images/pi.png?raw=true)


## Flash!
