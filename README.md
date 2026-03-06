[![License](https://img.shields.io/badge/License-Apache--2.0-green?label=License)](https://github.com/Arm-Examples/Blinky_FRDM-K32L3A6/blob/main/LICENSE)
[![Keil Studio Cloud - Import Project](https://img.shields.io/badge/Keil_Studio_Cloud-Import_Project-0091bd?logo=arm&logoColor=0091bd)](https://studio.keil.arm.com/?import=https://github.com/Arm-Examples/Blinky_FRDM-K32L3A6.git)
[![example workflow](https://img.shields.io/github/actions/workflow/status/Arm-Examples/Blinky_FRDM-K32L3A6/ci.yml?logo=arm&logoColor=0091bd&label=Example%20Publishable)](https://www.keil.arm.com/)
[![CMSIS Compliance](https://img.shields.io/github/actions/workflow/status/Arm-Examples/Blinky_FRDM-K32L3A6/verify.yml?logo=arm&logoColor=0091bd&label=CMSIS%20Compliance)](https://www.keil.arm.com/cmsis) 

# Blinky Project for FRDM-K32L3A6 (Cortex-M4)

The **Blinky** project can be easily used to verify the basic tool setup:

- In the beginning, `vioLED0` blinks in 1 sec interval.
- Pressing `vioBUTTON0` changes the blink frequency and start/stops `vioLED1`.
- `printf` messages are shown on the serial console.

The output of the serial console can be observed via the **SERIAL MONITOR** in Visual Studio Code.

Refer to [Project Configuration](#project-configuration) for board specific settings.


## Prerequisites

The following tools need to be installed on your machine:

- [CMSIS-Toolbox v2.12.0](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/releases) or newer
- [Microsoft Visual Studio Code](https://code.visualstudio.com/download) with
  [Keil Studio Pack](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack) extension (optional,
  alternatively [CLI](#using-command-line-interface-cli) can be used)
- [Arm Compiler 6](https://developer.arm.com/Tools%20and%20Software/Arm%20Compiler%20for%20Embedded) (automatically
  installed when using Visual Studio Code with vcpkg)

## Build solution

### Using Keil Studio

The following is written for [Keil Studio](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack), a
set of VS Code extensions.

Required tools described in file `vcpkg-configuration.json` should be automatically installed by vcpkg. You can see the
status of vcpkg in the status bar.

Required CMSIS packs need to be also installed. In case a required pack is missing, a notification window will pop-up
to install the missing pack.

Open the **CMSIS view** from the side bar and press the **Build** button.

### Using command line interface (CLI)

Download required packs (not required when the packs are already available) by executing the following commands:

```sh
csolution list packs -s Blinky.csolution.yml -m > packs.txt
cpackget update-index
cpackget add -f packs.txt
```

Build the project by executing the following command:

```sh
cbuild Blinky.csolution.yml
```

## Run and debug in Keil Studio

### Run

- Connect the board's OpenSDA USB to the PC (provides also power).
- Open the 'CMSIS' view from the side bar and press the 'Run' button and wait until the image is programmed and starts
  running.

### Debug

Open the **CMSIS** view from the side bar and press the **Debug** button. In the **Debug** view, use the
**SERIAL MONITOR** to connect to the board's serial port with 115200 baud rate and observe the output.

RTOS awareness is available through the **XRTOS** view in the bottom panel.

## Project Configuration

### Keil RTX5 real-time operating system

The real-time operating system [Keil RTX5](https://arm-software.github.io/CMSIS-RTX/latest/index.html) implements
the resource management.

It is configured with the following settings:

- [Global Dynamic Memory size](https://arm-software.github.io/CMSIS-RTX/latest/config_rtx5.html#systemConfig_glob_mem):
  2048 bytes
- [Default Thread Stack size](https://arm-software.github.io/CMSIS-RTX/latest/config_rtx5.html#threadConfig): 256 bytes
- [Idle Thread Stack size](https://arm-software.github.io/CMSIS-RTX/latest/config_rtx5.html#threadConfig): 128 bytes
- [Timer Thread Stack size](hhttps://arm-software.github.io/CMSIS-RTX/latest/config_rtx5.html#timerConfig): 256 bytes
- [Stack Overflow Checking](https://arm-software.github.io/CMSIS-RTX/latest/config_rtx5.html#threadConfig_ovfcheck) and
  [Stack Usage Watermark](https://arm-software.github.io/CMSIS-RTX/latest/config_rtx5.html#threadConfig_watermark)
  enabled

Refer to [Configure RTX v5](https://arm-software.github.io/CMSIS-RTX/latest/config_rtx5.html) for a detailed
description of all configuration options.

### CMSIS-Driver mapping

| CMSIS-Driver | Peripheral
|:-------------|:----------
| USART1       | LPUART1

| CMSIS-Driver VIO  | Physical board hardware
|:------------------|:------------------------------
| vioBUTTON0        | User Button (SW2)
| vioLED0           | User LED Red (RGB)
| vioLED1           | User LED Green (RGB)

### Other settings

**STDIO** is routed to a debug console through a virtual COM port (via OpenSDA, baudrate = 115200).
