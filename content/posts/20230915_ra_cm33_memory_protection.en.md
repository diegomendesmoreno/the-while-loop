+++
title = "How to Protect RA (Arm Cortex-M33) MCU Flash Memory against Reading"
date = "2023-09-15"
categories = ["renesas","ra","embedded"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
+++

## Introduction

During the production of an electronic product, it is desirable to protect the memory against reading after the firmware (binary) has been flashed to the microcontroller. This ensures that the firmware is not accessible to competitors and/or malicious hackers.

To protect the memory of the [RA family from Renesas](https://www.renesas.com/us/en/products/microcontrollers-microprocessors/ra-cortex-m-mcus) of microcontrollers, specifically those with an Arm Cortex-M33 core, it is necessary to use the Device Lifecycle Management (DLM). Both the debug capability and the serial programming interface depend on its configurations [1]. You can verify the Device Lifecycle states below [2]:

![Device Lifecycle States](../../../../../assets/img/20230915_dlm_states.png)

The transitions from one state to another are described below:

![Device Lifecycle State Transitions](../../../../../assets/img/20230915_dlm_transitions.png)

Note that it is possible to make reverse transitions, such as going from the `DPL` (Deployed) state of memory protected against reading, back to the `SSD` (Secure Software Development) state with memory reading access, via a key authentication process [3] [4]. If you have not performed the key authentication process, it is still possible to recover write and read access (reversing the transition), but all MCU memory will be erased, thus protecting the program content.


## Example

See below a step-by-step guide on how to write an example firmware and protect the memory without key injection. To do this, we will use _[Renesas Flash Programmer](https://www.renesas.com/us/en/software-tool/renesas-flash-programmer-programming-gui)_ to:
* Flash the example firmware
* Verify that it is possible to read the flashed firmware
* Make the transition from `SSD` to `NSECSD` and then to `DPL`
* Verify memory protection
* Revert to the `SSD` state and verify that the memory has been erased

### Flash the example firmware
Follow the steps in the article _[Getting Started with Renesas Flash Programmer](../getting-started-with-renesas-flash-programmer/)_ to write the example firmware that flashes the LEDs on the [EK-RA4M2 evaluation kit for RA4M2 MCUs](https://www.renesas.com/us/en/products/microcontrollers-microprocessors/ra-cortex-m-mcus/ek-ra4m2-evaluation-kit-ra4m2-mcu-group).


### Verify that it is possible to read the flashed firmware
Follow the steps in the article _[How to Read Microcontroller Memory with Renesas Flash Programmer](../how-to-read-microcontroller-memory-with-renesas-flash-programmer/)_ to read the flashed firmware.


### Make the transition from `SSD` to `NSECSD` and then to `DPL`
Go to `Target Device` >> `Read Device Information`, and verify that the microcontroller is in the initial state `SSD` (or `CM`).

![RFP Read Device Information SSD](../../../../../assets/img/20230915_rfp_read_device_ssd.png)

![RFP Device SSD](../../../../../assets/img/20230915_rfp_device_ssd.png)

Go to `Target Device` >> `DLM Transition...`, and choose `NSECSD`.

![RFP DLM Transition](../../../../../assets/img/20230915_rfp_dlm_transition.png)

![RFP DLM Transition to NSECSD](../../../../../assets/img/20230915_rfp_dlm_transition_nsecsd.png)

See now with `Read Device Information` that the device is in `NSECSD` state:

![RFP Device NSECSD](../../../../../assets/img/20230915_rfp_device_nsecsd.png)

Following the same process, go to `Target Device` >> `DLM Transition...`, and now choose `DPL`. And also verify with `Read Device Information`:

![RFP Device DPL](../../../../../assets/img/20230915_rfp_device_dpl.png)


### Verify Memory Protection
Although it is possible to read general device information with `Read Device Information`, it is no longer possible to read the flashed firmware. Like before, go to `Target Device` >> `Read Memory...` and try to read the first 8KB (0x2000) of the _Code Flash_. And as expected, it is not possible to read the flashed firmware:

![RFP Read Memory Fail](../../../../../assets/img/20230915_rfp_read_memory_fail.png)
`Error(E100000E): A protection error occurred in the device. (Command: 15, Response: D5)
Operation failed`


### Revert to the `SSD` state and verify that the memory was erased
Since we did not set up the key authentication process, to return to the initial `SSD` state, it is necessary to go to `Target Device` >> `Initialize Device`:

![RFP Initialize Device](../../../../../assets/img/20230915_rfp_initialize_device.png)

After this process, the LEDs stop flashing, indicating that the device memory has been erased. And, by doing a final `Read Device Information`, we verify that the device has returned to the `SSD` state and can be programmed again.

![RFP Device SSD](../../../../../assets/img/20230915_rfp_device_ssd.png)


## References

* [1]: [RA4 Quick Design Guide](https://www.renesas.com/us/en/document/apn/ra4-quick-design-guide)
* [2]: [Renesas RA Securing Data at Rest Using the Arm® TrustZone®](https://www.renesas.com/us/en/document/apn/renesas-ra-securing-data-rest-using-arm-trustzone)
* [3]: [Renesas RA Family Device Lifecycle Management Key Installation](https://www.renesas.com/us/en/document/apn/renesas-ra-family-device-lifecycle-management-key-installation)
* [4]: [How to Generate and Program DLM Keys for RA With SCE9](https://www.renesas.com/in/en/video/renesas-flash-programmer-tutorial-how-generate-and-program-dlm-keys-ra-sce9)
