# Blinky Project for FRDM-K32L3A6 (Cortex-M4)

The **Blinky** project can be easily used to verify the basic tool setup:

- In the beginning, `vioLED0` blinks in 1 sec interval.
- Pressing `vioBUTTON0` changes the blink frequency and start/stops `vioLED1`.
- `printf` messages are shown on the serial console.

The output of the serial console can be observed in a Terminal window on VS Code.

Refer to [Project Configuration](#project-configuration) for board specific settings.

[![Keil Studio Cloud - Import Project](https://img.shields.io/badge/Keil_Studio_Cloud-Import_Project-0091bd?logo=arm&logoColor=0091bd)](https://studio.keil.arm.com/?import=https://github.com/Arm-Examples/Blinky_FRDM-K32L3A6.git)
[![example workflow](https://img.shields.io/github/actions/workflow/status/Arm-Examples/Blinky_FRDM-K32L3A6/ci.yml?logo=arm&logoColor=0091bd&label=Example%20Publishable)](https://www.keil.arm.com/)
[![CMSIS Compliance](https://img.shields.io/github/actions/workflow/status/Arm-Examples/Blinky_FRDM-K32L3A6/verify.yml?logo=arm&logoColor=0091bd&label=CMSIS%20Compliance)](https://www.keil.arm.com/cmsis) 

## Prerequisites

### Tools:
 - [CMSIS-Toolbox v2.0.0](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/releases) or newer
 - [Microsoft Visual Studio Code](https://code.visualstudio.com/download) with Keil Studio Pack extension (optional, alternatively CLI can be used)
 - [Arm Compiler 6](https://developer.arm.com/Tools%20and%20Software/Arm%20Compiler%20for%20Embedded) (automatically installed when using Visual Studio Code with vcpkg)

## Build Solution/Project

### Using Visual Studio Code with extensions

Required tools described in file 'vcpkg-configuration.json' should be automatically installed by vcpkg. You can see the status of vcpkg in the status bar.

Required CMSIS packs need to be also installed. In case a required pack is missing, a notification window will pop-up to install the missing pack.

Open the 'CMSIS' view from the side bar, select desired 'Build Type' and press the 'Build' button.

### Using Command Line Interface (CLI)

Download required packs (not required when the packs are already available) by executing the following commands:
   ```sh
   csolution list packs -s hello.csolution.yml -m >packs.txt
   cpackget update-index
   cpackget add -f packs.txt
   ```
Build the project by executing the following command:
```sh
cbuild hello.csolution.yml
```

## Run the application

### Using Visual Studio Code with extensions

 - Connect the board's DAPLink USB to the PC (provides also power).
 - Open the 'CMSIS' view from the side bar:
   - Press the 'Run' button and wait until the image is programmed and starts running.
   - Press the 'Open Serial' button and select the board's serial port with 115200 baud rate.
 - Observe the terminal output.

 ### Using Drag-and-drop programming or external programmer and terminal

 - Connect the board's DAPLink USB to the PC (provides also power).
 - Program the image (.hex) using Drag-and-drop programming or use external programmer.
 - Open terminal on the PC and connect to the board's serial port with 115200 baud rate.
 - Observe the terminal output.

## Debug the application

### Using Visual Studio Code with extensions

Open the 'CMSIS' view from the side bar and press the 'Debug' button.

## Project Configuration

### RTOS: Keil RTX5 Real-Time Operating System

The real-time operating system [Keil RTX5](https://arm-software.github.io/CMSIS_5/RTOS2/html/rtx5_impl.html) implements the resource management. 

It is configured with the following settings:

- [Global Dynamic Memory size](https://arm-software.github.io/CMSIS_5/RTOS2/html/config_rtx5.html#systemConfig): 24000 bytes
- [Default Thread Stack size](https://arm-software.github.io/CMSIS_5/RTOS2/html/config_rtx5.html#threadConfig): 3072 bytes
- [Event Recorder Configuration](https://arm-software.github.io/CMSIS_5/RTOS2/html/config_rtx5.html#evtrecConfig)
  - [Global Initialization](https://arm-software.github.io/CMSIS_5/RTOS2/html/config_rtx5.html#evtrecConfigGlobIni): 1
    - Start Recording: 1

Refer to [Configure RTX v5](https://arm-software.github.io/CMSIS_5/RTOS2/html/config_rtx5.html) for a detailed description of all configuration options.

### Board: NXP FRDM-K32L3A6

The tables below list the device configuration for this board. The board layer for the NXP FRDM-K32L3A6 is using the software component `::Board Support: SDK Project Template: project_template (Variant: frdmk32l3a6)` from `NXP.FRDM-K32L3A6_BSP.13.0.0` pack.

The heap/stack setup and the CMSIS-Driver assignment is in configuration files of related software components.

The example project can be re-configured to work on custom hardware. Refer to ["Configuring Example Projects with MCUXpresso Config Tools"](https://github.com/MDK-Packs/Documentation/tree/master/Using_MCUXpresso) for information.

#### System Configuration

| System Component        | Setting
|:------------------------|:-------------------------------------------------------------
| Device                  | K32L3A60VPJ1A:cm4
| Board                   | FRDM-K32L3A6
| SDK Version             | ksdk2_0
| Heap                    | 64 kB (configured in linker script K32L3A60xxx_cm4*.scf file)
| Stack (MSP)             |  1 kB (configured in linker script K32L3A60xxx_cm4*.scf file)

#### Clock Configuration

| Clock                   | Setting
|:------------------------|:-------
| FIRC                    |  48 MHz
| FIRC DIV1 clock         |  48 MHz
| FIRC DIV2 clock         |  48 MHz
| FIRC DIV3 clock         |  48 MHz
| LPUART0 clock           |  48 MHz
| LPUART1 clock           |  48 MHz
| LPSPI0 clock            |  48 MHz
| LPI2C3 clock            |  48 MHz

**Note:** configured with Functional Group: `BOARD_BootClockRUN`

#### GPIO Configuration and usage

| Functional Group            | Pin | Peripheral | Signal   | Identifier         | Pin Settings                           | Usage
|:----------------------------|:----|:-----------|:---------|:-------------------|:---------------------------------------|:-----------------------------------
| BOARD_InitDEBUG_UART        | N2  | LPUART0    | TX       | DEBUG_UART0_TX     | default                                | LPUART0 TX for debug console (PTC8)
| BOARD_InitDEBUG_UART        | P3  | LPUART0    | RX       | DEBUG_UART0_RX     | default                                | LPUART0 RX for debug console (PTC7)
| BOARD_InitLEDs              | D6  | GPIOA      | GPIO, 24 | RGB_RED            | default                                | User LED1 (PTA24)
| BOARD_InitLEDs              | E6  | GPIOA      | GPIO, 23 | RGB_GREEN          | default                                | User LED2 (PTA23)
| BOARD_InitLEDs              | B6  | GPIOA      | GPIO, 22 | RGB_BLUE           | default                                | User LED3 (PTA22)
| BOARD_InitButtons           | B10 | GPIOA      | GPIO,  0 | SW2                | default                                | User Button SW2 (PTA2)
| BOARD_InitButtons           | P16 | GPIOE      | GPIO,  8 | SW3                | default                                | User Button SW3 (PTE8)
| BOARD_InitButtons           | N16 | GPIOE      | GPIO,  9 | SW4                | default                                | User Button SW4 (PTE9)
| BOARD_InitButtons           | L12 | GPIOE      | GPIO, 12 | SW5                | default                                | User Button SW5 (PTE12)
| BOARD_InitARDUINO_UART      | A5  | LPUART1    | TX       | ARDUINO_LPUART1_TX | default                                | Arduino UNO R3 pin D1 (PTA26)
| BOARD_InitARDUINO_UART      | B5  | LPUART1    | RX       | ARDUINO_LPUART1_RX | default                                | Arduino UNO R3 pin D0 (PTA27)
| BOARD_InitARDUINO_SPI       | C2  | LPSPI0     | SCK      | ARDUINO_SPI_SCK    | default                                | Arduino UNO R3 pin D13 (PTB4)
| BOARD_InitARDUINO_SPI       | D2  | LPSPI0     | SOUT     | ARDUINO_SPI_MOSI   | default                                | Arduino UNO R3 pin D11 (PTB5)
| BOARD_InitARDUINO_SPI       | E2  | LPSPI0     | SIN      | ARDUINO_SPI_MISO   | default                                | Arduino UNO R3 pin D12 (PTB7)
| BOARD_InitARDUINO_SPI       | E1  | GPIOB      | GPIO,  6 | ARDUINO_SPI_SSN    | Direction Output, GPIO initial state 1 | Arduino UNO R3 pin D10 (PTB6)
| BOARD_InitPins_Arduino_PTB3 | C1  | GPIOB      | GPIO,  3 | ARDUINO_PTB3       | Direction Input                        | Arduino UNO R3 pin D9  (PTB3)

#### NVIC Configuration

| NVIC Interrupt      | Priority
|:--------------------|:--------
| LPUART1             | 4
| LPSPI0              | 4

**STDIO** is routed to a debug console through Virtual COM port (DAP-Link, peripheral = LPUART0, baudrate = 115200)

#### CMSIS-Driver mapping

| CMSIS-Driver | Peripheral
|:-------------|:----------
| USART1       | LPUART1

| CMSIS-Driver VIO  | Physical board hardware
|:------------------|:------------------------------
| vioBUTTON0        | User Button SW2
| vioLED0           | User LED RED
| vioLED1           | User LED GREEN
