# SUNLU S9 with MKS Robin Nano V3

Marlin Firmware configured for SUNLU S9 with MKS Robin Nano V3 motherboard.  
Based on [Marlin](https://github.com/MarlinFirmware/Marlin) sources.

## Commit - Minimal installation

Configured as 'SUNLU S9+'  
'MKS Robin Nano V3' Minimum Configuration  
Stepper drivers A4988  
\+ LIN_ADVANCE

## Commit - MKS Lcd

Fixed observed bugs in MKS.  
At the end of a program, the elapsed time is displayed (from version 2.0.9.2)

## Commit - Direct drive

'BMG Dual Drive' extruder with 'Ender-3 CR10' head  
Calibrated by M303 E0 S250 C8 U for 'Ender-3 CR10' head  
3 materials in Preheat Constants. (on MKS_TS35_V2_0 only 2 work)

## Commit - Stepper drivers TMC2209

Marlin error in LIN_ADVANCE:

```c++
//#define EDGE_STEPPING
//Already enabled by default but interferes with TMC2209
//in older versions it is //#define SQUARE_WAVE_STEPPING but now if we disable it it also asks:
#define NO_EDGE_STEPPING_WARNING
```

\+ More diagnostics

## Commit - Allow international symbols in long filenames

But MKS TS35 V2 0 has problem with Cyrillic fonts.

### Commit - Indicator MKS_MINI_12864_V3

The test is successful. For now I only need it for backup. But they say it works with Klipper too.

## Commit - Back to MKS_TS35_V2_0

### Other tests

can be replaced:

```c++
//#define TFT_CLASSIC_UI
//#define TFT_COLOR_UI
#define TFT_LVGL_UI
```

but without TFT_LVGL_UI there is no MKS_WIFI_MODULE

## Commit - Fast Z, max temperature extruder for ABS = 280

## Commit - Mount BIGTREETECH UPS 24V V1.0

## Commit - Merge remote-tracking branch 'upstream/release-2.1.3-beta3'

## Commit - Improving the bed leveling grid

## Commit - OctoPrint

- With these settings in **Marlin** it works from **SD ​​card** and from **OctoPrint**. We can safely start printing from 'PC' or 'LCD' and forget about the computer. The filament sensor is in **MKS Robin Nano V3**, there is no need to connect to **Raspberry PIO**. No additional plugins or settings are required in **OctoPrint**. All control is in **Marlin** duplicated by host notifications and commands in **OctoPrint**;
- For pauses (for example, for installing nuts) can be built into 'Gcode' **M25** or **M125** (M125 allows stopping at certain coordinates, for example: 'M125 X0 Y300 L0 P1');
- For changing the filament can be built into 'Gcode' **M600**, which first unloads the filament;
- Set the parameters after '#define ADVANCED_PAUSE_FEATURE' and '#define NOZZLE_PARK_FEATURE', according to the specific printer;
- After 'PURGE' from 'LCD' or 'OctoPrint', the extruder stepper motor is turned on and the filament cannot be manipulated manually through the extruder gears;
- It also worked well with the 'Change_Filament' plugin;
- I read about many changes of:

```c++
#define FILAMENT_RUNOUT_SCRIPT "M600"
```

with similar:

```c++
#define FILAMENT_RUNOUT_SCRIPT "M118 //action:pause"
#define FILAMENT_RUNOUT_SCRIPT "M412 H"
```

but all lead to settings in **OctoPrint** and incompatibility of printing from **OctoPrint** and **SD ​​card**.

### Additionally

- I gave up on **TFT_LVGL_UI**, switched to **TFT_COLOR_UI** and subsequently lost **MKS_WIFI_MODULE (esp3d)**. 'MKS' stopped developing the software 4-5 years ago and M25, M125 and M600 do not work properly. With TFT_COLOR_UI everything works fine with SD card. If we work without SD card, only with OctoPrint can MKS - TFT_COLOR_UI be used, but forget about filament sensors and pauses without plugins.

- this publication was very useful to me:
https://www.reddit.com/r/3Dprinting/comments/l6xei2/how_to_detect_filament_runout_in_marlin_and/
