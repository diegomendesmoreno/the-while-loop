+++
title = "Getting Started with Renesas Flash Programmer"
date = "2023-09-05"
categories = ["Renesas","RA","Embedded"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
+++

## Introduction

The [Renesas Flash Programmer](https://www.renesas.com/us/en/software-tool/renesas-flash-programmer-programming-gui) is a firmware programming tool that allows you to write data to the flash memory of Renesas microcontrollers.

## Download and Installation
Go to the [software page in the Downloads section](https://www.renesas.com/us/en/software-tool/renesas-flash-programmer-programming-gui#download), login to your Renesas account (create one if necessary), then download and install the tool.

## Writing Example Firmware
To validate the installation, we will write example firmware to a board with an _RA_ (ARM Cortex M33) microcontroller. Open the _Renesas Flash Programmer_, create a new project with the following settings and connect:

![RFP Configuration](../../../../../assets/img/20230905_rfp_config.png)

Make sure that the board has connected successfully:

![RFP Connected](../../../../../assets/img/20230905_rfp_connected.png)

Choose the firmware image that will be written:

![RFP Choosing .srec](../../../../../assets/img/20230905_rfp_choosing_srec.png)

Click _Start_:

![RFP Start](../../../../../assets/img/20230905_rfp_start.png)

Verify that the firmware has been written successfully:

![RFP Firmware Download success](../../../../../assets/img/20230905_rfp_firmware_download_success.png)
