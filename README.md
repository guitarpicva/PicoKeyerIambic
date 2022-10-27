Copyright 2022 GrizzWorks, LLC - All Rights Reserved
# THIS CODE IS NOT READY FOR USE!  IT CURRENTLY FUNCTIONS AS A COOTIE!
Pico Keyer Iambic

Pico Keyer Iambic is shared in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

------------------------------------------------------------------
NOTE: See the guitarpicva/PicoKeyerTerminal.git project
on github for a simple serial terminal to use with the keyer.
------------------------------------------------------------------

Pico Keyer Iambic is a simple Morse code keyer project to demonstrate
how to use a GPIOs on the RPi Pico microcontroller to key a radio's 
iambic keyer.

The "build" folder has been included with the binary .uf2 file
for evaluation convenience.  The ".vscode" folder is also 
included for those who wish to build the project in VS Code
themselves.

The Pico's onboard LED is keyed to demonstrate the keyed letter 
and punctuation and prosigns.  See alphabet.h for the full list 
of implemented characters.  In addition, the GPIO pin 6 
(Pico physical pin 9) and GPIO pin 7 (Pico physical pin 10) are
also keyed for use with two keying circuits.  The "R" sent at 
startup (Ready) and speed changes are only sent to the LED.  

Two keying circuits are highly recommended.  A simple transistor switch,
such as a 2N2222A with an appropriate bias resistor (~1k ohm) connected
between the GPIO pin and the transistor's base is likely all that
would be needed as a switch between ground and the radio's key 
line.  If clicking is encountered in the RF output, the use of 
opto-isolated transistors may be necessary.  A device such as 
a 4N25 or VO610A may be appropriate.

The keying is treated as one would use a "iambic key", meaning
a dot switch and a dash switch.  Thus the radio settings
should be for using an iambic key and key jack wiring as
required by the radio manufacturer.  Typically, the "tip" of the 
radio's key jack would be connected to the Emitter of the DOT transistor 
switch, and the Ground of the radio's key jack would be connected 
to the Collector of the transistor.  So when the GPIO pin turns
the transistor switch to ON, the key line will be grounded and
the keyer will be sent a DOT pulse.  A second transistor switch
wired the same way to the DASH GPIO pin (7) would send a DASH pulse
to the keyer.

The above demonstrates the most common radio keying implementation 
in radios or code practice oscillators. Please ensure that your 
radio's keyer works this way before designing your switching circuits.

To supply the ASCII characters for the Pico to key, UART0 is
used as a serial input device.  Characters are echo'd back on the
port so the user may see what has been typed into the Serial
terminal or UI application.

If an RPi is used as the input computer, one of it's UARTs may
be directly wired into the Pico on pins 1 (TX) and 2 (RX). The
code has commented lines which deal with the setup and use of
the USB instead of the UART.  Please also make the adjustments in
the CMakeLists.txt file as indicated for use of a USB.  Also note
that the USB use only seems reliable when used with an RPi
computer.  Windows gives wildly varying results.  

Use of a UART and an external serial cable will necessitate the
use of an inexpensive UART to RS-232 conversion module.  These
are very inexpensive and turn the UART into an RS-232 serial port.
That serial interface can then be used with a computer serial port
or attached to a USB to RS-232 converter cable.


NOTE: the below may be phased out for the Iambic Keyer version

Speed of keying is specified by an input command to the USB.
The format "@nn" is used to set the keying speed to "nn" words
per minute based on the formula for WPM using the "PARIS " word
methodology.  

The formula is :  

     T = 1200 / W

where T = time for a single "dot" in milliseconds and W = "PARIS " 
Words per minute.

When the users sends "@12" for example, the keyer will adjust it's
"dot" time to 1200/12 = 100 ms.

Future designs may also include an "iambic" keyer for radios 
which have electronic keyers built into them.  In effect, this 
is a keyer keying a keyer!  But it provides the convenience of 
using a keyboard instead of an iambic key, which for movement 
impaired people may be welcome.