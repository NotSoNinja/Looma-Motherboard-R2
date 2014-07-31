LOOMA motherboard readme

How to use this manual: combine information here with information found in the datasheets for 
the individual components and in your EE degree.

Disclamer: this manual's joking tone is not indicative of its content.

Contents
=========================================
1. Surge Stopper
2. 5v Buck Circuit
3. 19.5v Boost Circuit
4. Audio Amplifier
5. USB Hub
6. Fan Control
7. Ethernet
8. Known Issues
9. Debugging
=========================================

1. THE SURGE STOPPER
-----------------------------------------
As the name suggests, the surge stopper stops surges.  The circuit is based on the LTC4364-2 IC.
It uses two mosfets in series to stop surges up to 200V.  The currently implemented circuit is
based on the first example circuit under TYPICAL APPLICATION in the datasheet, entitled: 4A, 12V 
Overvoltage Output Regulator with Ideal Diode Withstands 200V 1ms Transient at VIN.  We modified 
that circuit to tolerate 10A and stop surges at a much lower threshold (12.5V-13V as opposed to 30V)
by tweaking the current sense and feedback resistors.  As a bonus, this design withstands having its
input shorted to ground.

WARNING: Previous iterations of this circuit were known to catch fire.


2. 5V BUCK CIRCUIT
-----------------------------------------
The Buck circuit takes a 12V input from the surge stopper and turns it into a steady 5V to power the
Odroid CPU and USB ports.  This circuit seems to have been designed based on the "Adjustable Type
Circuit" found under "Typical Application Circuit (Continued)" in the AP1501A datasheet.  This is
one of the few things on the motherboard that, so far, has not caught fire.


3. 19.5V BOOST CIRCUIT
-----------------------------------------
This problem child of a circuit *should* be fixed now.  It takes an input of 12V from the Surge
Stopper and boosts it to satisfy a 19.5V 3A output requirement.  During its initial boost, the 
circuit draws a full 10A from its supply, but this only lasts about 5-10ms.  During normal 
operation, it draws more in the range of 6A.  The resistor in series with the output IS necessary, 
as if the circuit sees no load, it tries to amplify voltage to infinity and beyond (did I mention
that this circuit has caught fire before?).  The new circuit is based on an example circuit entitled
"12V to 48V 50W Step-Up Converter with 400kHz Switching Frequency" found at the end of the LT3844
datasheet.  Current operation is overdamped, but the output voltage is a stable 19.5V by 12ms of 
operation.


4. AUDIO AMPLIFIER
-----------------------------------------
This circuit is almost entirely encompassed by TPA1517DWPR, an audio amplifier IC from TI.  The
capacitors make sure AC noise is eliminated from the DC supply, and DC offset is eliminated from the 
audio in and out signals.


5. USB HUB
-----------------------------------------
Based on the TI TUSB2077A chip, the seven-port USB hub has four external ports and three internal 
ports.  The hub is "self-powered" (it takes its power from the 5V line), and individually regulates 
the 5V outputs on each port via two TPS2044 IC's.  The hub is minimalist, without transient 
eliminators or other chip manufacturer "recommended" bits.  The 3.3V supply for TUSB2077A is 
provided by an LT1963 IC.


6. FAN CONTROL
-----------------------------------------
Current design is just a mosfet opened and closed by the Odroid U3's onboard fan control.


7. EHERNET
-----------------------------------------
This extremely complex circuit uses state of the art dual layer PCB traces and precision manufacturing
to make what is essentially a right-angle connector using two ethernet ports.  There is literally no 
signal processing here.


=========================================
KNOWN ISSUES
=========================================

-Things catch fire
-Things melt.
-As a side effect of the above, power supply is not constant.
-Not enough space
-Short in 19.5V port


=========================================
DEBUGGING
=========================================
-This section left intentionally blank-