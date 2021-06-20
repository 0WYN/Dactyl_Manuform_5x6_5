# Dactyl Manuform 5x6_5
*A 62 key split columnar ergonomic keyboard build log by Adam Halfpenny*

![DACTYL_MANUFORM](https://i.imgur.com/Wu5byPh.jpeg)

The Dactyl Manuform is a 3D printed handwired keyboard running QMK Firmware on a Pro Micro controller. Here is a build log of how to make one:

Print the case using a 0.4 mm nozzle - supports are needed - I suggest the 'tree' style of supports to conserve material and aid in ease of removal.

| ![](https://i.imgur.com/yl66v7f.jpegg) | ![](https://i.imgur.com/qCnb8ht.jpeg) |![](https://i.imgur.com/YfCkNEn.jpeg) |
|-----------------------------|------------------------------|------------------------------|

Depending on the tolerances of your printer, you may want to use hot glue to secure the switches and ensure they don't make a rattling sound. Press in the brass inserts with a soldering iron.


# Handwiring guide

Once the switches are secure - it's time for soldering. First secure the diodes on the Amoeba PCBs, then solder to the switches. Once there's an Amoeba PCB on each switch, connect the rows and columns according to [this wiring diagram](https://miro.medium.com/max/1050/1*Q7xYKNrfMr8au7zUipBlGg.jpeg). (colour coded columns on my unit)

| ![](https://i.imgur.com/FbVfrDT.jpeg) 	| ![](https://i.imgur.com/bqOUol5.jpeg) 	| ![](https://i.imgur.com/gHkgkrK.jpeg) 	|
|-----------------------------|------------------------------|------------------------------|

Flash the Pro Micro before soldering to ensure it is functional beforehand (my configuration files included in the repo and flashing instructions below). Then cut each wire to the length required to reach each row or column and solder into the unit. (Again referring to the wiring guide above)
| ![](https://i.imgur.com/SU8jQuO.jpeg) 	| ![](https://i.imgur.com/dcodaNX.jpeg) 	| ![](https://i.imgur.com/uDrDyCO.jpeg) 	|
|-----------------------------|------------------------------|------------------------------|

You need to connect from Pro Micro to the back of the unit with a USB Micro male to USB-C female adaptor cable.  Unfortunely they are not readily available, so I improvised and made my own!
| ![](https://i.imgur.com/sLyY97a.jpeg) 	| ![](https://i.imgur.com/1TAhQcg.jpeg) 	| ![](https://i.imgur.com/K7jOM31.jpeg) 	|
|-----------------------------|------------------------------|------------------------------|

USB Micro 2.0 to USB-C daughterboard pinout as follows:

    BLACK   GREEN   WHITE   RED
    G       D+      D-      V

Then it is just a case of connecting any jumper wires to their corrosponding pins on the micro controller (Such as the pins going to the TRRS jack for I2C connection). Finally hot glue the TRRS jack and USB-C port into their corrosponding holes.

(Optional extra) Connect a momentary switch to the RST + GRD pins on the Pro Micro for flashing in the future.

![](https://i.imgur.com/N6MZnGv.jpeg)
# Bill Of Materials

* 62 x diodes
* 62 x cherry mx style switches
* 62 x Amoeba PCBs ([keeb.io](https://keeb.io/products/amoeba-single-switch-pcbs)) (optional - make soldering easier)
* 24 AWG (0.2 mm2) wire / jumper wires
* 2 x Pro Micro
* 2 x TRRS Jacks
* 2 x Reset switch
* 10 x M3x5 countersunk screws to attach base plates
* 10 x M3 Brass threaded inserts
* hot glue for securing the pro micro to the case (optional, but recommended)

# Pin assignment

    ROW0    ROW1    ROW2    ROW3    ROW4    ROW5
    F6      F7      B1      B3      B2      B6
    
    
    COL0    COL1    COL2    COL3    COL4    COL5
    D4      C6      D7      E6      B4      B5


    I2C
    D0 + VCC + GRD


# QMK Firmware Flashing
After configuring your QMK environment according to [the QMK setup procedure](https://beta.docs.qmk.fm/tutorial/newbs_getting_started) and downloading my configuration from this repo you can flash the Pro Micro with this command:

    qmk flash -kb handwired/dactyl_manuform/5x6_5 -km 0WYN

# Flashing EE_HANDS eeproms from /qmk_firmware/quantum/split_common
This allows the use of each side individually through USB, otherwise the right side thinks it is a left but with a flipped matrix.

    avrdude -p atmega32u4 -P /dev/tty[YOUR_DEVICE] avr109 -U eeprom:w:eeprom-righthand.eep
    avrdude -p atmega32u4 -P /dev/tty[YOUR_DEVICE] avr109 -U eeprom:w:eeprom-lefthand.eep