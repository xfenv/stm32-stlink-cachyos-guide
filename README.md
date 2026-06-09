# STM32 Programming with ST-Link on CachyOS (Arch-based Linux) **[WIP]**
This guide provides a practical, step-by-step workflow for setting up basic STM32 development using an ST-Link debugger, STM32CubeMX for initial code generation, and VSCode for development. The goal is to provide the required resources and help with the basic setup on Linux, while addressing common issues such as USB detection, permissions (**udev** rules), and ST-Link connectivity.

### Prerequisites
  * Basic familiarity with embedded systems (optional but helpful).
  * Basic command-line usage (navigating directories, running commands).

### Disclaimer
  * All commands and configurations in this guide are provided as-is. You are responsible for any changes made to your system.
  * If you are not familiar with the command line, you should still be able to follow along, but you should **always review and understand commands before executing them**. Running commands with elevated privileges (e.g. `sudo`) can modify system configuration and may have unintended consequences.


## Operating System
### Arch-based systems
This guide was tested on **CachyOS**, but it should work with little to no modification on other Arch-based Linux distributions as well, as they share the same package manager (`pacman`) and system structure.

### Debian-based systems
As there are differences in package installation, package names, and file locations (e.g. for udev rules), the exact steps may differ. However, the concepts and fixes presented in this guide should still translate well to Debian-based systems.


## Used Hardware
The setup described in this guide was tested using the following hardware:
  * **Debugger / ST-Link:** [STM32 Nucleo-144 development board](https://www.st.com/en/evaluation-tools/nucleo-f767zi.html) (STM32F767ZI MCU), which includes an integrated ST-Link programmer/debugger.
  * **Target device:** STM32F103C8T6 MCU on a [Blue Pill development board](https://stm32-base.org/boards/STM32F103C8T6-Blue-Pill.html).

**Note:** This setup should also work with a standalone ST-Link and other STM32 MCU combinations that support SWD.


## Prerequisite Software
You need to download the following software either directly or from the [AUR](https://wiki.archlinux.org/title/Arch_User_Repository). I used the official ST installer for STM32CubeMX and the AUR for the [Microsoft version of VSCode](https://wiki.archlinux.org/title/Visual_Studio_Code).

 * **[STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html):** Used for MCU configuration and generating the initial code.
 * **[VSCode](https://code.visualstudio.com/):** Main development environment.
 * **[STM32CubeIDE Extension for VSCode](https://marketplace.visualstudio.com/items?itemName=stmicroelectronics.stm32-vscode-extension):** Integrates STM32-specific functionality into VSCode.

**Important:** When using the AUR, always review the PKGBUILD before installing any package.


## Generating Code (CubeMX)


## Detecting ST-Link and ST-Link DFU (firmware updater)


## Connecting ST-Link to MCU


## Building and Flashing the Project


## Additional Troubleshooting
