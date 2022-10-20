# retro-hd6302-sbc-barta
Construction notes for a minimalist microcomputer based on the HD6303 IC.

## Introduction

This board was published on [Seed Studio](https://www.seeedstudio.com/Hitachi-HD6303-Single-Board-Computer-(SBC)-g-1017489) in September 2017 by a user called Barta. The actual author is unknown (its not me) , but I'll add proper attribution when I discover it.

I have reverse engineered the schematic and assembled the board. 

The integrated circuit runs in multipexed address/data bus mode as a microcomputer with a full 64k of accessible memory.
The 32K x 8 EPROM contains the Lilbug monitor program and will eventually contain a 6800 Figforth.
The 32K x 8 RAM chip uses a skinny DIP package and its hiding under the EPROM, 
soldered directly to the PCB.

The board exposes an 8-bit parallel IO port on an edge connector.
Another 3 bits (these set the mode) and some control signals are exposed on another edge connector.

The 4.9152 MHz crystal is one of those annoying odd frequencies required to obtain a standard baudrate.

A common USB to serial module is used. 
The pinout of these modules is not stardardised so the module is connected via ribbon cable.

Power to the board is supplied via the edge connection through a 5V regulated wall wart.
This is a temporary measure during construction as power during normal operation 
is derived from the USB-TTL UART configured to provide 5V.

### Initial configuration
![IMG_5484 2](https://user-images.githubusercontent.com/1712402/196090757-239bce5f-099a-4dab-af43-f08a50b5a755.jpg)

### Final configuration

![IMG_5498](https://user-images.githubusercontent.com/1712402/196854200-3afc4944-1a5d-4e9e-a85e-cff66e803f4b.JPG)

## Serial Communications

The micromputer to USB dongle pin association is as follows:

| Microcmputer (4 pins) | USB dongle (6 pins) |
| --------------------- | ------------------- |
| 5V                    | 5 V |
| GND                   | GND |
| Rx                    | Tx |
| Tx                    | Rx |

The monitor assembly code has been has been edited to provide 9600 baud instead of the default 300 baud.

On a Macintosh or other unix-like desktop computer the terminal can be used to identify the USB dongle.

`
ls -al /dev/tty*
`

This produces a result that looks like this:

`
/dev/tty.usbserial-AB0LZ57U
`

This only produces a result if the USB dongle is plugged into a USB socket on the desktop computer. 
Then the connection to the "HD6303 Single Board Computer" can be established by issuing a command like:

`
screen /dev/tty.usbserial-AB0LZ57U 9600
`

Pressing the reset button on the SBC should produce a welcome message.
Issuing a Lilbug command such as

`
D F800 F900
`
Produces this result:

![Screen Shot 2022-10-20 at 3 36 19 pm](https://user-images.githubusercontent.com/1712402/196857619-6397128e-36a6-4e02-8bd9-75d331dd4b4c.png)

Lilbug on the Macintosh does not understand form-feed or backspace/delete.
