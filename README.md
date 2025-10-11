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

- При тези настройки в **Marlin** работи работи от **SD card** и от **OctoPrint**. Може спокойно да запуснем печата от 'PC' или 'LCD' и да забравим за компютъра. Датчика за филамента е в **MKS Robin Nano V3**, няма нужда да се свързва към **Raspberry PIO**. Не са необходими никакви допълнителни плъгини или настройки в **OctoPrint**. Цялото управление е в **Marlin** дублирано от хост уведомления и команди към **OctoPrint**;
- За паузи (например за вграждане на гайки)  могат да се вграждат в 'Gcode' **M25** или **M125** (M125 разрешава спиране на определени координати като пример: 'M125 X0 Y300 L0 P1');
- За смяна на филамента могат да се вграждат в 'Gcode' **M600**, която педварително разтоварва филамента;
- Настройте параметрите след '#define ADVANCED_PAUSE_FEATURE' и '#define NOZZLE_PARK_FEATURE', според конкретния принтер;
- След 'PURGE' от 'LCD' или 'OctoPrint' се влючва стъпковия двигател на екструдера и не ноже да се манинипулира филамента ръчно чрез зъбчатките на екструдера;
- Добре се сработи и с плъгина 'Change_Filament';
- Прочетох за много смени на:

```c++
#define FILAMENT_RUNOUT_SCRIPT "M600"
```

с подобни:

```c++
#define FILAMENT_RUNOUT_SCRIPT "M118 //action:pause"
#define FILAMENT_RUNOUT_SCRIPT "M412 H"
```

но всички водят до настройки в **OctoPrint** и несъвместимост на печата от **OctoPrint** и **SD card**.

### Допълнително

- Отказах се от **TFT_LVGL_UI**, преминах на **TFT_COLOR_UI** в последствие загубих **MKS_WIFI_MODULE (esp3d)**. 'MKS' са прекратили развитието на софтуера преди 4-5 години и не работят правилно M25, M125 и M600. При TFT_COLOR_UI всичко работи добре с SD card. Ако работим без SD card, само с OctoPrint може да се ползва и MKS - TFT_COLOR_UI, но забравете за датчици за филаменти и паузи без плъгини.

- от голяма полза ми беше публикацията:
https://www.reddit.com/r/3Dprinting/comments/l6xei2/how_to_detect_filament_runout_in_marlin_and/
