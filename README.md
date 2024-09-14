# BSPD (Brake System Plausibility Device)

![V4.0](Documents/BSPD_V4_Render.png)

[**FULL SCHEMATIC**](Documents/BSPD_V4_Schematic.pdf)


## Working Principle

The diagram below shows how the bspd would work (NOT the actual schematic)

![alt text](<Documents/BSPD Logic.jpg>)

- If both sensors' voltage are above the threshold voltage (set using the potentiometer) **or** SCS is triggered -> the bspd will cut off power.
- The delay only triggeres when the input signal is ON for the whole duration (eg, a 200ms signal will be rejected by the 500ms delay)
- The bspd will automatically reset once the triggering signal is no longer present for 10s


## Troubleshooting

There are 3 LEDs and 6 testpoints to be used when troubleshooting

- Sensor 1 or 2 LED turns ON 
  - **CAUSE:** The sensor voltage of 1 or 2 is higher than the threshold
  - Adjust the potentiometer to set the correct voltage threshold while measuring the voltage on TP1 and TP2 (Voltage on the test points should change as the potentiometer is turned).
  - Measure the input sensor voltage (Make sure the sensor is working)

- SCS LED turns ON
  - **CAUSE:** One of the sensors' voltage are outside the range of 0.5V to 4.5V
  - Make sure both sensors are connected (if one of the sensor is disconnected it will trigger the SCS).
  - Measure both sensors voltage to be within the range of 0.5V to 4.5V
  - Try to connect a 2.5V signal to both of the sensor input (The LED should turn off).
  - If all fails, there might be a fault on the scs circuit

- BSPD not resetting after 10s
  - Disconnect anything connected to TP6 (if theres any)
  - Make sure none of the LED is on during the 10s duration
  - Try to test the normal operation of the bspd to ensure that only the 10s delay is not working (chances are there might be other fault that is causing this)


## Important Notes

- For test point 6, TP6 (10s Delay Voltage), probing the point will affect the duration of the 10s delay. This is due to the use of a 1 Mega ohm resistor which is the range of most oscilloscope/multimeter input resistance. Using a 1X probe with an oscilloscope can even prevent the delay from working at all. So, **do not use the TP6 to measure the delay**, instead just use TP5 which is essentially the delay output.
- When manufacturing the board with the uni, have fun finding super small hidden shorted traces (possibly one under silkscreen). For some reason most of the BSPD boards made by the uni always have their own faults so, save yourself from a future headache by testing out the board slowly as you assemble them. **Removing copper pours might help so please remove it for the next revision.**


## Design Choices

Some parts of the schematic might need some extra explanation:

- The comparator LM339 can only reliably compare signals if one signal above 3V (refer the datasheet, look at "common mode range"), so to check for the 4.5V threshold, a voltage divider is used to scale down the input by half.
- The outputs of the comparator is connected together for SCS check since it is an open-drain output which basically OR gate all the outputs together
- The 10s timer uses a 555 monostable retriggerable circuit which we just realised that there is a dedicated IC that does the same thing. Something like SN74LVC1G123, which you might consider for future revision.


## Version Changelog

**V1.0**
- First Routed PCB

**V1.1**
- Changed R26 to 4.7k to limit current to base of the BJT controlling the relay

**V2.0**

- Changes are made after feedback from Dr. Theo
- Changed 500ms timer resistor to 430k with the assumption of 1% tolerence (Expected = 516ms, Min = 511ms, Max = 521ms)
- Changed the current limiting resistors from 470 to 4.7k to limit current to less than 1 mA (NAND gate limit)
- Removed copper pad on the mounting holes and added clearance from other copper pads
- Fixed ground routing loop on the PCB
- Smaller hatched copper pours
- Removed redundant voltage divider for SCS voltage reference

**V3.0**
- Replaced 5V supply input with 12V supply input
- Added 5V Voltage Regulator with capacitors
- Team logo added

**V4.0**
- Changed 5V Reg to a more common LM7805
- Changed transistors footprint to a bigger seperation pads footprint due to uni fabrication limitation
- Changed trimmer to a bigger but more common equivalent alternative
- Added diode for reverse relay coil voltage protection 
- PCB Routing optimised
- Added test points for easier troubleshooting