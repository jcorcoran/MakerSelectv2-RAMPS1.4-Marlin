# Hardware configuration
This project is the Marlin firmware configuration for a Monoprice Maker Select v2.1 (https://www.monoprice.com/product?p_id=13860) 3D printer with after market customizations.

The stock configuration of this printer had voltage on male header pins for the extruder fan. While performing maintenance, with the fan unplugged, 12VDC  from the fan header pins contacted the chassis (grounded). This in turn blew out stepper drivers on the Melzi board. The driver ships are soldered directly to the main PCB, so instead of replacing the whole PCB ($$$) or getting the hot air rework station warmed up, I decided to do a complete conversion of the control electronics to a RAMPS 1.4 board.

## Stepper Motors (Stock)
X, Y, E - MOONS C17HD40102-01N
Z - 2x MOONS C17H06039-06N

## Control / Electronics
RAMPS 1.4 
DRV8825 stepper drivers
FDP6035 MOSFET driving the heated bed - FET on the RAMPS 1.4 was getting uncomfortably warm. Alternatively 
DRV8825 on X, Y, E (1/32 steps)
A4988 on Z, (1/16 steps)

## Extruder
Micro Swiss all metal hotend for Wanhao i3


# Marlin 3D Printer Firmware
https://github.com/MarlinFirmware/Marlin
