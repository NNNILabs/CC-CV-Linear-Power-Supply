# CC CV Linear Power Supply
## Clean and elegant CC CV linear supply, no weird biasing or floating stuff.

I designed this power supply to replace the hacked-together ATX/buck-boost/LT3081 I was using. 
The goal from the beginning was simplicity and to use parts that are easily available, such as those from the JLCPCB parts library.

It is "modular" in the sense that only the op-amps and the associated feedback circuitry were meant to be JLC assmebled, with the MOSFETs, input/output electrolytics, current shunts and protection diodes being added by the user.

Since the parts and topology used are rather straightforward, this thing can even be built on a perfboard (I tried, it worked).

Capabilities:
- Output voltage - 0V to 30V, with "true" 0V (load and MOSFET leakage dependent)
- Output current - 0A to (500mV/Rshunt)A, maximum (limited by parts on the board) 6A per rail, with both MOSFETs populated
- Input voltage - up to 50V

**Important - to make full use of the output voltage and current ranges, the MOSFETs *must* be heatsinked.**

Circuit details:
- 5V for op-amps and reference: 2x 5V regulators to reduce heat dissipation, with an input pre-regulator based on the TL431 so that the 5V regulator's maximum input voltage is not exceeded (in this case 24V) as well as to spread the power dissipation between more parts to reduce temperature rise. Jumper J1 can be shorted if the input voltage is below the maximum rated input voltage for the 5V regulator being used, this reduces input quiescent current.
- Buffered references: The output voltage and current are set using two potentiometers. Since the potentiometers are connected to 5V, the reference voltages for CC and CV are 0V to 5V. The potentiometer outputs are buffered using a dual RRIO op-amp to prevent loading (and therefore errors). This means that the output voltage and current are *independent* of the potentiometer value, but make sure that the current consumed by both potentiometers does not exceed 1mA (or a minimum potentiometer resistance of 5K).
- +CV - straightforward op-amp circuit that compares the CV reference to an output divider
- +CC - a differential amplifier that forms a Howland current source, threshold set by CV reference
- -CV and -CC are not very straightforward - CV forms a "null detector" that tries to maintain 0V at the summing junction of the CV reference and output voltage through appropriately scaled resistors, while the CC differential amplifier sets the summing junction voltage, increasing it if the current limit is reached and therefore reducing output voltage
- MOSFET gate driver - a slightly modified "AB" amplifier to increase the op-amp output current enough to drive the MOSFET gate to higher voltages.
