---
title: Openocd On Ubuntu 64
redirect_from:   
- /Openocd.html
---
 This How to covers, if I rip my usb how do I flash the Proffieboard or update it. there's a few things about this particular use case. In this article you will be using the ST-Linkv2 usb debugger that connects through single core and the operating system of your laptop or pc would be Ubuntu 64 bit or any linux distro that is 64 bit. 
 
 on a windows box these steps are not required because of the openocd files are located in the arduino ide folders. the main issue with Ubuntu 64 bit version is that the scripts on arduino require the 32 bit version and ubuntu has deprecated libudev.so.1, THERE IS A FIX THOUGH.
 
 ```
 ./openocd.bin: error while loading shared libraries: libudev.so.1: cannot open shared object file: No such file or directory
 ```
 
 pre-requisites
 the hardware required is the ST-Linkv2 debugger USB, plus the male/female pinned cables and small pcb test points connect the female pins into the usb pins that correspond with 1. sdio 2. swdlck and 3 gnd. the male end will go to the corresponding pins on the proffieboard. use the pcb test points to hold the pins in place other wise the connection may be lost during the compiling stage. 
 
 1. Verify configuration in Arduino IDE and locate the file (ProffieOS.ino.elf) in the /tmp folder this can be found if you have debug enabled in the arduino preferences for the option of compiling. copy the hole output from arduino and then paste into a text file and run a search on the filename. e.g.  "/tmp/arduino_build_610194/ProffieOS.ino.elf"
 2. navigate to Terminal and Install openocd 
    ```
    # sudo apt-get install openocd
    ```
 3. navigate to the  directory (please note that the number in the directory is randomly assigned and yours will be different)
 ```
    # cd /tmp/arduino_build_610194/ 
 ```
 4. in the terminal run the following command
 
 ```
 openocd -f /home/max/.arduino15/packages/proffieboard/hardware/stm32l4/3.6.0/variants/STM32L433CC-Proffieboard/openocd_scripts/stm32l433cc_butterfly.cfg  -c “program ./ProffiOS.ino.elf verify reset ext”
 ```
 
 a successfuly compiling looks like this.
 
 
 ```
 root@Heironymous:/tmp/arduino_build_265075# openocd -f /home/max/.arduino15/packages/proffieboard/hardware/stm32l4/3.6.0/variants/STM32L433CC-Proffieboard/openocd_scripts/stm32l433cc_butterfly.cfg -c "program ProffieOS.ino.elf verify reset exit"
 Open On-Chip Debugger 0.11.0
 Licensed under GNU GPL v2
 For bug reports, read
 	http://openocd.org/doc/doxygen/bugs.html
 WARNING: interface/stlink-v2.cfg is deprecated, please switch to interface/stlink.cfg
 Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
 none separate
 
 Info : clock speed 500 kHz
 Info : STLINK V2J29S7 (API v2) VID:PID 0483:3748
 Info : Target voltage: 3.280851
 Info : STM32L433.cpu: hardware has 6 breakpoints, 4 watchpoints
 Info : starting gdb server for STM32L433.cpu on 3333
 Info : Listening on port 3333 for gdb connections
 Info : Unable to match requested speed 500 kHz, using 480 kHz
 Info : Unable to match requested speed 500 kHz, using 480 kHz
 target halted due to debug-request, current mode: Thread 
 xPSR: 0x01000000 pc: 0x1fff3f36 msp: 0x20002c20
 ** Programming Started **
 Info : device idcode = 0x10016435 (STM32L43/L44xx - Rev Z : 0x1001)
 Info : flash size = 256kbytes
 Info : flash mode : single-bank
 Warn : Adding extra erase range, 0x08038a68 .. 0x08038fff
 ** Programming Finished **
 ** Verify Started **
 ** Verified OK **
 ** Resetting Target **
 Info : Unable to match requested speed 500 kHz, using 480 kHz
 Info : Unable to match requested speed 500 kHz, using 480 kHz
 shutdown command invoked
 ```
