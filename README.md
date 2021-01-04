# Hardware configuration
This project is the Marlin firmware configuration for a [Monoprice Maker Select v2.1](https://www.monoprice.com/product?p_id=13860) 3D printer with after market customizations.

The stock configuration of this printer had voltage on male header pins for the extruder fan. While performing maintenance, with the fan unplugged, 12VDC  from the fan header pins contacted the chassis (grounded). This in turn blew out stepper drivers on the Melzi board. The driver chips are soldered directly to the main PCB, so instead of replacing the whole PCB ($$$) or getting the hot air rework station warmed up, I decided to do a complete conversion of the control electronics to a RAMPS 1.4 board.

## Stepper Motors (Stock)
* X, Y, E - MOONS C17HD40102-01N
* Z - 2x MOONS C17HD6039-06N

## Control / Electronics
* RAMPS 1.4 
* FDP6035 MOSFET driving the heated bed - FET on the RAMPS 1.4 was getting uncomfortably warm. Alternatively 
* DRV8825 stepper drivers on X, Y, E (1/32 steps)
* A4988 stepper driver on Z, (1/16 steps)
* SN04-N proximity sensor for auto bed leveling
* Stock LCD - wired to [RAMPS LCD adapter](https://robotdyn.com/pub/media/0G-00005284==RAMPS1_4-adapter/DOCS/PINOUT==0G-00005284==RAMPS1.4-adapter.jpg)  
  
  ```
               _____
          GND | 1 2 | 5V
     (BEEPER) | 3 4 | RESET
    (BTN_ENC) | 5 6   (LCD_D4)
    (BTN_EN2) | 7 8 | (LCD_EN)
    (BTN_EN1) | 9 10| (LCD_RS)
               -----
                LCD
  ```
  
  Note, this wiring can be simplified using a firmware mod, so that only one EXP connector needs to be wired into w/ a single ribbon cable.
  [similar to that used in the SKR fork](https://github.com/jcorcoran/MakerSelectv2-SKR1.3-Marlin2.0/blob/main/Marlin/Marlin/src/pins/lpc1768/pins_BTT_SKR_V1_3.h#L293)


## Extruder
Micro Swiss all metal hotend for Wanhao i3


# Marlin 3D Printer Firmware
https://github.com/MarlinFirmware/Marlin


# Cura

## Startup gcode:

```
G21 ;metric values
G90 ;absolute positioning
M82 ;set extruder to absolute mode
M107 ;start with the fan off

M140 S{material_bed_temperature_layer_0} ; set bed temp
M104 S160 ;set extr preheat temp

G28 X0 Y0 ;move X/Y to min endstops
G28 Z0 ;move Z to min endstops

M117 Wait BED temp
M190 S{material_bed_temperature_layer_0} ; wait for bed final temp
M117 Auto level bed
G29 ; Run auto bed leveling

;wait for extruder temp to get back up to setpoint
;it gets turned off for periods of the leveling script
M117 Wait EX. temp
M109 S{material_print_temperature_layer_0} ; wait for extruder final temp
M117 Wait BED temp
M190 S{material_bed_temperature_layer_0} ; wait for bed final temp
M117 ...

G1 Z15.0 F9000 ;move the platform down 15mm
G92 E0 ;zero the extruded length
G1 F200 E3 ;extrude 3mm of feed stock
G92 E0 ;zero the extruded length again
G1 F9000
M117 Printing...
```

## End GCode

```
M104 S0 ;extruder heater off
M140 S0 ;heated bed heater off (if you have it)
G91 ;relative positioning
G1 E-1 F300  ;retract the filament a bit before lifting the nozzle, to release some of the pressure
G1 Z+0.5 E-5 X-20 Y-20 F9000 ;move Z up a bit and retract filament even more
G28 X0 Y0 ;move X/Y to min endstops, so the head is out of the way
M84 ;steppers off
G90 ;absolute positioning
```
