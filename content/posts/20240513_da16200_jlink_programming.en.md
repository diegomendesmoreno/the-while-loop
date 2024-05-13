+++
title = "Flashing/Programming the DA16200 with J-Link"
date = "2024-05-13"
categories = ["Renesas","RA","Embedded","Connectivity"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
draft = false
+++

## Introduction

Whether you're developing a custom application or using the [AT Command Set](https://www.renesas.com/us/en/software-tool/da16200-wi-fi-command-set) firmware image, it is necessary to flash/program it inside the either the [DA16200](https://www.renesas.com/us/en/products/wireless-connectivity/wi-fi/low-power-wi-fi/da16200-ultra-low-power-wi-fi-soc-battery-powered-iot-devices) SoC or the [DA16200MOD](https://www.renesas.com/us/en/products/wireless-connectivity/wi-fi/low-power-wi-fi/da16200mod-ultra-low-power-wi-fi-modules-battery-powered-iot-devices) module.

The stadard way to do this is through the built-in serial bootloader. And to do that you need to follow the steps in section _4.5 Programming Firmware Images_ of the document [DA16200 DA16600 FreeRTOS Getting Started Guide](https://www.renesas.com/us/en/document/qsg/da16200-da16600-freertos-getting-started-guide?r=1600096). It is important that you connect the COM Port the debug interface (UART0) and not the ATCMD interface (UART1).

However, if you want to flash/program those same images through the J-Link interface, there are a few steps you need to do that are described below.



## Requirements

- Download and install the [J-Link software](https://www.segger.com/downloads/jlink/)
- Download the `DA16200 DA16600 FreeRTOS SDK` in the _Software Downloads_ section of the DA16200 or DA16200MOD page.
- Have Python Installed.
- Have the `BOOT` and `RTOS` images you want to flash.
  - This example uses the provided images at the `DA16200 DA16600 FreeRTOS SDK Image` package, also available in the in the _Software Downloads_ section of the DA16200 or DA16200MOD page.



## Pre-work before flashing/programming

### Get the images

- Go to inside the `DA16200 DA16600 FreeRTOS SDK Image` package, in the `DA16200_IMG_FreeRTOS_ATCMD_UART1_EVK_v3.2.8.1_4MB` folder, and copy the `DA16200_FBOOT-GEN01-01-c7f4c6cc22_W25Q32JW.img` and `DA16200_FRTOS-GEN01-01-2aa77df370-006629.img` images to `\DA16200_DA16600_SDK_FreeRTOS_v3.2.8.1\utility\programming-scripts`.

### Add the DA16200 device inside J-Link's device list

- Go to the J-Link folder (i.e.: `C:\Program Files (x86)\SEGGER\JLink`), and add the following `XML` code to end of the `JLinkDevices.xml` file, before the closing `</DataBase>` tag:
```xml
  <!-- -->
  <!-- Dialog Semiconductor -->
  <!-- -->
  <Device>
  <ChipInfo Vendor="Dialog Semiconductor" Name="DA16200_W25Q32JW" Core="JLINK_CORE_CORTEX_M4" WorkRAMAddr="0x80000" WorkRAMSize="0x00040000" />
   <FlashBankInfo Name="QSPI Flash" BaseAddr="0x100000" MaxSize="0x400000" Loader="Devices/Dialog/DA16200_W25Q32JW.elf" LoaderType="FLASH_ALGO_TYPE_OPEN" />
  </Device>
  <Device>
   <ChipInfo Vendor="Dialog Semiconductor" Name="DA16200.SeggerES.4MB" Core="JLINK_CORE_CORTEX_M4" WorkRAMAddr="0x83000" WorkRAMSize="0x00020000" />
   <FlashBankInfo Name="QSPI Flash" BaseAddr="0x10000000" MaxSize="0x400000" Loader="Devices/Dialog/ES_DA16200_4MB.elf" LoaderType="FLASH_ALGO_TYPE_OPEN" />
  </Device>
  <Device>
   <ChipInfo Vendor="Dialog Semiconductor" Name="DA16200.SeggerES.2MB" Core="JLINK_CORE_CORTEX_M4" WorkRAMAddr="0x83000" WorkRAMSize="0x00020000" />
   <FlashBankInfo Name="QSPI Flash" BaseAddr="0x10000000" MaxSize="0x200000" Loader="Devices/Dialog/ES_DA16200_2MB.elf" LoaderType="FLASH_ALGO_TYPE_OPEN" />
  </Device>
  <Device>
   <ChipInfo Vendor="Dialog Semiconductor" Name="DA16200.eclipse.4MB" Core="JLINK_CORE_CORTEX_M4" WorkRAMAddr="0x83000" WorkRAMSize="0x00020000" />
   <FlashBankInfo Name="QSPI Flash" BaseAddr="0x10000000" MaxSize="0x400000" Loader="Devices/Dialog/DA16200_4MB.elf" LoaderType="FLASH_ALGO_TYPE_OPEN" />
  </Device>
  <Device>
   <ChipInfo Vendor="Dialog Semiconductor" Name="DA16200.eclipse.2MB" Core="JLINK_CORE_CORTEX_M4" WorkRAMAddr="0x83000" WorkRAMSize="0x00020000" />
   <FlashBankInfo Name="QSPI Flash" BaseAddr="0x10000000" MaxSize="0x200000" Loader="Devices/Dialog/DA16200_2MB.elf" LoaderType="FLASH_ALGO_TYPE_OPEN" />
  </Device>
```
- Go to `\DA16200_DA16600_SDK_FreeRTOS_v3.2.8.1\utility\j-link\scripts\flashloader\Devices` and copy the `Dialog` folder with the flashloader files to the `Devices` folder inside the J-Link folder.



## Procedure

More details at `\DA16200_DA16600_SDK_FreeRTOS_v3.2.8.1\utility\programming-scripts\ReadMe.txt`):

- Make one combined binary with the images
```bash
python mkimage.py -o output.bin .\DA16200_FBOOT-GEN01-01-c7f4c6cc22_W25Q32JW.img 0x0 .\DA16200_FRTOS-GEN01-01-2aa77df370-006629.img 0x23000
```

- Erase chip's memory:
```bash
python programmer.py -p "JLINK_PATH" chip_erase
```

- Flash/Program the combined binary file:
```bash
python programmer.py -p "JLINK_PATH" write_bin output.bin
```



References:
- [Getting Started with the DA16200 FreeRTOS SDK | learn.sparkfun.com](https://learn.sparkfun.com/tutorials/getting-started-with-the-da16200-freertos-sdk/all)
- [DA16200 programming with JTAG](https://community.renesas.com/wireles-connectivity/f/wi-fi/20036/da16200-programming-with-jtag/93612#93612)
