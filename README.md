# SKR2-Marlin-2.0.9.3-Delta<br>
First Edition: 14-02-2022<br>      

Updates: 23-02-2022<br>

Description<br>
=========<P>
I have an AnyCubic Kossel Pulley 3D printer since April 2017. It has been working well. However, in August 2021, the 8-bit Trigorilla board died, hence initiating a motherboard replacement. On the same month, I got hold of an SKR 2 Rev B board, four TMC2209 v1.2 stepper motor drivers and a BTT ESP8266 WIFI Module ESP-12S. All other parts remain more or less the same. The firmware installed is 2.0.9.2-BugFix.<P>

On top of the 240mm diameter heating bed (2.9 ohm at 29.5C = 4.1A current requirement) is a 3mm Bolo glass with 200mm diameter. Endstops are original mechanical two wires type. Stepper motors are 42HD4027-01 NEMA x 4 units. Display is a RepRap Discount Smart Controller (EXP1 and EXP2) c/w SDCard slot.<P>

From 14-02-2022 onwards, I upgrade the firmware to the latest 2.0.9.3 stable version. Below were the changes that I made to the relevant configuration files to get the printer up. Hope this helps someone, as I was helped many times by other contributors.<P>

Marlin Configuration - for AnyCubic Kossel Pulley DELTA Printer<P>

Description: Upgrading from Marlin version 2.0.9.2 (Bugfix)  to 2.0.9.3 (Stable). Some of the information that I added, like the (J) Bonus is another way to help me remember what I did or can do when I hit similar problems later.<P>

Marlin Firmware<br>
===========<P>

Downloaded Marlin firmware -2.0.9.3 from https://marlinfw.org/meta/download/<br>
- the downloaded file is Marlin-2.0.x<br>
- I renamed and unzipped this main file to Marlin-2.0.9.3. For easier identification later on<P>

Downloaded Configuration Examples from https://github.com/MarlinFirmware/Configurations/releases/tag/2.0.9.3<br>
- steps:<br>
- scroll down to bottom of page<br>
- click on "Source code (zip)" and download the file, which is named "2.0.9.3.zip"<br>
- unzip the file and drill down to "Examples" folder >Anycubic >Kossel<br>
- copy the files "Configuration.h & Configuration_adv.h"<br>
- paste them into >- Marlin-2.0.9.3>Marlin-2.0.x >Marlin  (overwrite the existing configuration files)<P>

Running Visual Studio Code 1.64.2<br>
=========================<P>
Load up VSC<br>
- if you see previous version 2.0.9.2 and do not want to see them on the screen, click File and close all the folders.<br>
- select File > Open folder and open up the Marlin -2.0.9.3 folder as per this sequence below:<br>
  >Marlin-2.0.9.3 >Marlin-2.0.x >Marlin [you should not see folders "lib" and "src"]<P>

OR at this point in time, you may just CREATE a new project<br>
- Load up the PIO Home page<br>
- create a new Project [+ New Project]<br>
- make sure you use the pop-up "File Explorer" window, to drill to the Marlin folder.<br>
- click on the "Select" folder button.<br>
- wait for about 2 seconds and all will be well<P>

If you want to compare this Configuration.h with the current 2.0.9.2, visually, do this:<br>
- click on the "Split Editor" icon at the top-right of the VSC screen.<br>
- now you will see two copies of the same Configuration.h. Ignore this 2nd copy - let it be here for a while. You will close this later.<br>
- Click on the main menu option, File >Open File and look for the folder that keeps the 2.0.9.2 version of the Configuration.h. Select the 2.0.9.3 Configuration.h file.<br>
- this 2.0.9.2 Configuration.h would now be presented in this split window. You can move your cursor to the redundant 2nd Configuration.h file and click on the "x" to close it.<br>
- with this, you should be able to compare the 2.0.9.3 against the 2.0.9.2 for customised edit.<br>
- once you are done with the changes, close the Split Editor.<P>

If you find that, for some reasons, the PlatformIO:Build icon has disappeared from the bottom-left status bar, do this. Righ-click on the status bar and select PlatformIO:Toolbar.<br>

Contents Index<br>
===========<P>
A. Configuration.h<br>
B. Configuration_adv.h<br>
C. platformio.ini<br>
D. pins_BTT_SKR_V2_0_common.h<br>
E. Wiring of my existing Delta's Mechanical Endstops<br>
F. Installation of the BTT ESP8266 ESP-12S Module<br>
G. Compilation of BTT ESP8266-ESP-12S Wifi module [4MB]<br>
H. ESP3D-WEBUI / ESP3D-2.1.1<br>
I.  How to upload firmware 2.0.9.3 to Anycubic Kossel Printer<P>

A) Changes to Configuration.h<br>
======================<P>
line 33 #define ANYCUBIC_PROBE_VERSION 2 (default: 0)<br>
line 39 #define ANYCUBIC_KOSSEL_ENABLE_BED 1 (default: 0)<br>
line 107 #define MOTHERBOARD BOARD_BTT_SKR_V2_0_REV_B<br>
line 118 #define SERIAL_PORT 3 (from 0 to "3", for Wifi module)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- wifi won't work if reversed with SERIAL_PORT_2<br>
line 140 #define SERIAL_PORT_2 -1 (enabled - for USB conection)<br>
line 141 #define BAUDRATE_2 250000  (enabled)<br>
line 155 #define CUSTOM_MACHINE_NAME "ANYCUBIC Kossel SKR2"<br>
line 498 #define TEMP_SENSOR_0 5 (default)<br>
line 507 #define TEMP_SENSOR_BED 5 (default)<br>
line 798 //#define DELTA_HOME_TO_SAFE_ZONE (disabled)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- disabled so that print head will not go down like 50mm after homing<br>
line 814 #define DELTA_CALIBRATION_DEFAULT_POINTS 4 (No change)<br>
line 849 #define DELTA_HEIGHT 293.33   (default: 320)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- from manual Z-movement to bed check<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- should give 310.13 at LCD when Auto Home<br>
line 959 #define X_DRIVER_TYPE TMC2209 (default: A4988)<br>
line 960 #define Y_DRIVER_TYPE TMC2209 (default: A4988)<br>
line 961 #define Z_DRIVER_TYPE  TMC2209 (default: A4988)<br>
line 970 #define E0_DRIVER_TYPE TMC2209 (default: A4988)<br>
line 1069  #define DEFAULT_ACCELERATION         600 (from 3000)<br>
line 1071 #define DEFAULT_TRAVEL_ACCELERATION   1000 (from 3000)<br>
line 1139 //#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN (disabled)<br>
line 1317  #define NOZZLE_TO_PROBE_OFFSET { 0, 0, -17.175 }<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- default was -16.80<br>
line 1475 #define INVERT_Y_DIR false (from true)<br>
line 1476 #define INVERT_Y_DIR false (from true)<br>
line 1477 #define INVERT_Y_DIR false (from true)<br>
line 1485 #define INVERT_E0_DIR false (default:true)<br>
line 1693 #define AUTO_BED_LEVELING_UBL (enabled)<br>
line 1702 #define RESTORE_LEVELING_AFTER_G28 (enabled)<br>
line 1744 #define G26_MESH_VALIDATION (enabled)<br>
line 1748 #define MESH_TEST_HOTEND_TEMP  230 (default:205) <br>
line 1749 #define MESH_TEST_BED_TEMP      100 (default:60)<br>
line 1777 #define ABL_BILINEAR_SUBDIVISION (left enabled)<br>
line 1793 #define MESH_INSET 15   (default:1)<br>
line 1895 #define Z_SAFE_HOMING (enabled)<br>
line 1903 #define HOMING_FEEDRATE_MM_M { (35&#42;60), (35&#42;60), (35&#42;60) } <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- (from 100&#42;60,100&#42;60,100&#42;60)<br>
line 2018 #define PREHEAT_1_FAN_SPEED  255   (from 0) <br>
line 2021#define PREHEAT_2_TEMP_HOTEND 230 (from 240)<br>
line 2022 #define PREHEAT_2_TEMP_BED    100 (from 110)<br>
line 2024 #define PREHEAT_2_FAN_SPEED    0 (default)<br>
line 2037 #define NOZZLE_PARK_FEATURE (enabled)<br>
line 2163 //#define PRINTCOUNTER (disabled) -else out of memory during compiling<br>
line 2254 #define SDSUPPORT (so that we can use the SDcard slot)<br>
line 2261 //#define SD_CHECK_AND_RETRY (disabled) -else out of memory during compiling<p>
line 2356 #define REPRAP_DISCOUNT_SMART_CONTROLLER (I am using the old stock Controller)

B) Changes to Configuration_adv.h<br>
=========================<p>
line 511 #define USE_CONTROLLER_FAN (enabled)<br>
line 513 #define CONTROLLER_FAN_PIN FAN2_PIN (enable & set to FAN2_PIN. Default was -1)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- this means I am using PB5. You can type in PB5 or FAN2_PIN<br>
line 517 #define CONTROLLERFAN_SPEED_ACTIVE    190  (from 255)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- I am setting about half-strength so that it is not too noisy (old fan)<br>
line 519 #define CONTROLLERFAN_IDLE_TIME    20    (From 60)<br> 
line 611 #define E0_AUTO_FAN_PIN FAN1_PIN   (Default: -1) (for Hotend FAN1, see line 620)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- using FAN1_PIN instead of PB6 is easier to read<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- as FAN_PIN is default defined for FAN0 -Parts Fan BUT<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- make sure Cooling Fan Number in Cura is set to "0" otherwise no fan during print<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- I have an enclosure yet still need to enable 20% FAN0 else soggy printing.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- COOLER_AUTO_FAN_PIN is for laser fan use only<br>
line 1633 #define SDCARD_CONNECTION LCD<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- so that myRepRap Discount Controller can read the SDCard<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- otherwise it reads from the SKR2 board mSDCard!!<br>
line 2801   #define E0_CURRENT      950 (FROM 800)<p>

C) Changes to Platformio.ini<br>
=====================<P>
line 16 default_envs = BIGTREE_SKR_2<P>

D) For pins_BTT_SKR_V2_0_common.h [folder: Marlin/src/pins/stm32f4]<br>
=====================================================<P>
Commented out line 41 "#define FLASH_EEPROM_EMULATION"<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- This causes Marlin to use back your internal SD card as the persistent storage, with the created file EEPROM.dat<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- just make sure you have an SD Card in yur SDCard slot at all times. You should be as that is where you keep your gCode files<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- otherwise, you will get "No eeprom" error message when doing M500 or "Store Settings"<P>
  
I added #define Z_MIN_PIN Z_STOP_PIN below<br>
line 128 //<br>
line 129 // Z Probe (when not Z_MIN_PIN)<br>
line 130 //<br>
line 131 #ifndef Z_MIN_PROBE_PIN<br>
line 132 #define Z_MIN_PROBE_PIN PE4<br>
line 133 #define Z_MIN_PIN Z_STOP_PIN<br>
line 134 #endif<P>

E) Wiring of my existing Delta's Mechanical Endstops<br>
===================================<P>
Common and NC (default Anycubic 2017) wiring, therefore I am using GND/PC1(X), GND/PC3(Y), GND/PC0(Z) on the SKR 2.0 board<P>

F) Installation of the BTT ESP8266 ESP-12S Module<br>
======================================<P>
Please refer to the SKR2_ESP8226_Delta github for installation steps.<P>
  Here: https://github.com/walterlootk/SKR2_ESP8226_DELTA <P>

G) Compilation of BTT ESP8266-ESP-12S Wifi module [4MB]<br>
=================================<P>
Please refer to the SKR2_ESP8226_Delta github for installation steps.<P>
  Here: https://github.com/walterlootk/SKR2_ESP8226_DELTA <P>

H) ESP3D-WEBUI / ESP3D-2.1.1<br>
=================<P>
Please refer to the SKR2_ESP8226_Delta github for installation steps.<P>
Here: https://github.com/walterlootk/SKR2_ESP8226_DELTA <P>

I) How to upload firmware 2.0.9.3 to Anycubic Kossel Printer<br>
====================================<P>
If you don't see the "PlatformIO:Build" tick mark at the bottom-left of the task bar, you have to load up the PIO Home  page and create a new Project [+ New Project], making sure you use the pop-up "File Explorer" to drill to the Marlin folder. Remember, the correct Marlin folder is the one where you can see the "lib" & "src" folders.<P>

First, compile the amendments you made to the Configuration.h, Configuration_adv.h and other dependant files, by clicking the "PlatformIO:Build" check-mark at the bottom-left of the VSC taskbar.<P>

When there is no compilation errors, copy the "firmware.bin" file into your mSDCard. The "firmware.bin" file is located at >marlin-2.0.9.3>Marlin-2.0.x>.pio>build>BIGTREE-SKR_2.<P>

Insert the mSDCard into the SKR2 motherboard mSD Slot and power up the Kossel.<P>

If you get a [Ignore] or [Reset] prompt, click on the [Reset] and reBOOT the Kossel. All came out fine, with the Wifi Module up as well.<P>

You do not need to re-install the Wifi firmware, since you have done it to the SKR2 at the previous 2.0.9.2 Marlin version.<P>

Remember to do: M502, M500 or reset/init eeprom on display<P>
  
J) Bonus<br>
=======<P>
If you made some changes to the bed or shifted the endstops, you can make changes to the Z-Probe Offset, old school way.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Do an Auto Home, and note the Z-Height value at the LCD<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Take note of your Z-Probe Offset value. Example -16.80<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- If you are printing with ABS, set your bed temperature, e.g. 100C, first.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- It is okay to let the nozzle remain unheated.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Without mounting the probe, lower the print head down >Motion >Axis >Z-Axis, until it tugs gently (not hard, almost loose) at your A4 paper.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- If necessary, disable Soft Endstops, (from ON to OFF), to allow for the nozzle to go to negative value.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Take note of the Z-probe variance value from the A4 paper probing, e.g. -0.25<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Do this calculation: (Existing Z-probe Offset) + (new Z-Probe variance value)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  => (-16.80) + (-0.25) = -17.43 [this is math, so the +/- signs is important]<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Do a Store Settings.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Power off/power on the Delta, to be sure.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Finally, repeat this process(s) until you are satisfied<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- This is old school way, and you only do it once.<br>
  
K) Enabling UBL<br>
==============<p>
Initially I had some problems. The MESH Print came out with two close spots above the center, unprinted. It is just not sticking. I solved it (through friends help):<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Tightened the 4 screws (that hold the hotend securely) in place, as two of which had slightly loosen<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Flipped the glass over, just in case I have a bad glass<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Found out that it was actually caused by the Y & Z belts. Both are not guitar-string tight. Once tightened, no more sticking issue as well as the layer running with big gaps at certain times.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Use the LCD Step-byStep UBL guide instead of just issuing all the associated G29/G26 commands as per Youtube guide. This forces the printer to prime out a long filament trail and it helps to clear the nozzle beautifully, thus printing a fuller MESH instead of a skinny MESH.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- TAKE NOTE: The Manual Priming (from the LCD), although automatic, will continue to prime out the filament UNTIL you click the knob to stop it!!<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Also, before  you click on the Validate MESH, please put in your sticky stuff on your bed, if you need to. Pre-heat your bed, if you need to. And, by now, you should have removed your PROBE, if there is a need to, like mine.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- I also did not include the G29 L1 code into the Cura printer Start Code. The printer knows that it has already been activated.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- The few times I use the Wifi Module to issue commands are for G29 T (list the table), G29 S1 and M500 (no need for these two commands as this storing handled in the LCD as well.<br>
  
L) Visual Source Code<br>
==================<p>
If you have previously installed VSC and suddenly, due to installation of a new program or there was a jert with your computer, you can try the following to get your projects in<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Click on File > Open Folder<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Drill down to the Marlin folder - the one that contains the SRC and LIB folders, and click the Select folder button.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Your previously working project(s) will come back to live.                                                                                                                                               
