# Introduction
- Simple SDcard SDIO 4 bit data logging system
- STM32F407VG
- You should install the necessary dependencies for this project in order to build (original project dependencies aren't included to this repository to reduce the project's size). 
Creating a new project using CubeMX and included `hal_sdio_4bit.ioc` configuration is recommended.
# Todos:
- Periodically check if SD card is present, if not don't do the logging.
- Ardupilot: ARM -> start logging, should implement similarly base on the idea that system should only log when commanded, otherwise we could mistakenly lose the log files.
- Code for a more consistent logging system: how to add a variable to log system, logged data UID, sample rate,....
- Implement benchmark testing to the system.
- Write variable names at the first row
- Write system information to log file (firmware version, frame), date. Or we could utilize to log the messages from the vehicle.
# Explanation
SDcard are used in many applications:
+ High speed (storage device like EEPROM support I2C, SPI, therefore it’s limited to those protocols’ speeds (3.4Mb/s max for SPI), most of EEPROMs don’t support >1Mb/s).  
+ Some applications: Ardupilot’s storage for data logging file, Raspberry/Beaglebone storage for the OS. Camera

ST's library: stm32f4xx_sd.c, has typedef, enum, functions to be used in bsp_driver_sd.c to create some functions: like SD_get_card_info, HAL_SD_Erase, necessary typedefs, SDIO_TypeDef from stm32f4xx.h, the stm32f4xx_sd.c also get some struct typedefs from `stm32f4xx_ll_sdmmc.h`, `stm32f4xx_ll_sdmmc.h` in HAL driver is actually `stm32f4xx_sdio.h` in SPL driver
+ fatfs by Chan is written in ANSI C, therefore it's platform-independent.
+ file `diskio.c` will use some function from #include `"stm32f4_sdio_sd.h"`, which tell us about the relationship between the library and the MCU's hardware!
+ http://thehackerworkshop.com/?p=1103
+ SDCard: like an USB used for storage, but when considering ... SDCard has advantages: smaller than USB, and its consistency

# References
+ Run OK second SPL SDIO SDcard Project: C:\Users\Admin\Downloads\sdcard_ws\STM32F4-master\STM32F4 Discovery Software Examples\STM32F4xx_SDIO_Example\Project\FatFs\MDK-ARM
- https://github.com/mfauzi/STM32F4/tree/master/STM32F4%20Discovery%20Software%20Examples/STM32F4xx_SDIO_Example/Project/FatFs
+ format the SDcard: File system: FAT32 (Default). Allocation unit size: 4096 bytes.
+ Xuan thiep SDcard SDIO tutorial: 16. SD Card With SDIO FATFS
https://www.youtube.com/watch?v=52azToQwl7o&t=162s
- https://github.com/afinello/stm32f4xx-sdio-dma-driver
- https://www.youtube.com/watch?v=0NbBem8U80Y
- https://community.st.com/s/feed/0D50X00009XkW2dSAF : STM32 with SD Card and FatFs using CubeMX � Write error and write speed - Solution
- original name: FATFS-DBG_Keil.zip
Source: https://community.st.com/s/question/0D50X00009XkZzBSAV/sdio-and-sd-fat-access-example
SPL SDIO tested run ok
## SDcard speed test code:
https://github.com/zephyrproject-rtos/zephyr/issues/22906
https://github.com/MaJerle/stm32f429/blob/master/00-STM32F429_LIBRARIES/fatfs/drivers/fatfs_sd_sdio.c
From this code, we can see that Majerle is using the same by-passing method for sdio-4bit working as my current HAL code.

## Logging projects:
https://github.com/d-ronin/openlager
https://github.com/tudelft/highspeedlogger

## Documents
- https://www.st.com/resource/en/user_manual/dm00105259-developing-applications-on-stm32cube-with-fatfs-stmicroelectronics.pdf
- http://elm-chan.org/fsw/ff/00index_e.html
- https://community.st.com/s/question/0D50X00009XkhhjSAB/sdio-fatfs-speed-problem
