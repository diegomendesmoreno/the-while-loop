+++
title = "How to Read Microcontroller Memory with Renesas Flash Programmer"
date = "2023-09-10"
categories = ["renesas","ra","embedded"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
+++

## Introduction

This article assumes that you have followed the steps described in the article _[Getting Started with Renesas Flash Programmer](../getting-started-with-renesas-flash-programmer/)_ which writes an example firmware that blinks the LEDs on the [EK-RA4M2](https://www.renesas.com/us/en/products/microcontrollers-microprocessors/ra-cortex-m-mcus/ek-ra4m2-evaluation-kit-ra4m2-mcu-group) board.


## Reading Microcontroller Memory
With Renesas Flash Programmer open and connected to a microcontroller that has already been programmed (without memory protection against reading), go to `Target Device` >> `Read Memory...` and name the binary file (`read.mot`) that will be read:

![RFP Read Memory](../../../../../assets/img/20230910_rfp_read_memory.png)

Since the example program is small, we will only read the first 8KB of the Code Flash:

![RFP Read Memory Config](../../../../../assets/img/20230910_rfp_read_memory_config.png)

And we can see that the reading was successful:

![RFP Read Memory Success](../../../../../assets/img/20230910_rfp_read_memory_success.png)
