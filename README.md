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
### 1. Create a New Project
1. Open **STM32CubeMX**.
2. On the main page, find **New Project** and select the appropriate option. Since this guide targets a specific MCU, select **ACCESS TO MCU SELECTOR**.
3. From the MCU/Board selector, find your MCU/Board and click **Start Project**.

### 2. Configure Project
For this example, the MCU will be configured to use a single GPIO output. The Blue Pill board has a LED connected to **PC13**, which will be used.
1. Select **System Core** -> **RCC** -> **High Speed Clock (HSE)** -> **Crystal/Ceramic Resonator**.
2. Select **System Core** -> **SYS** -> **Debug** -> **Serial Wire**
3. In the **Pinout view**, click on **PC13** and select **GPIO_output**.

### 3. Configure Clock
Although this simple example doesn't require maximum performance, it is still useful to configure the system clock.
1. Navigate to **Clock Configuration**.
2. Set **HCLK (MHz)** to the maximum value (typically **72 MHz** for STM32F103).
3. Let STM32CubeMX calculate the configuration automatically, or adjust it manually if needed.

### 4. Generate Code
1. Navigate to **Project Manager**.
2. Name the Project and select a project folder.
3. Select **CMake** under **Toolchain/IDE**.
4. Click **Generate Code**.


## Detecting ST-Link and ST-Link DFU (firmware updater)
In order to program using ST-Link, your user account must have read and write access to the device. When installing the STM32CubeIDE extension for VSCode, necessary **udev** rules are not automatically added. This means they must be configured manually.

To do this:
1. Connect the ST-Link to your PC.
2. Open a terminal and list the USB devices: `lsusb`.
3. Find the ST-Link from the list and note the ID in the format `xxxx:xxxx` (for example, the Nucleo-144 uses `0483:374b`).
4. Create a file named **71-st-link.rules** in `etc/udev/rules.d/` with the following content:
   ```
   # STM32 ST-LINK/V2.1 on STM32F767ZIT6U dev board
   ACTION!="remove", SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", MODE="0660", TAG+="uaccess"
   ```
   **Note:** `ATTRS{idVendor}` and `ATTRS{idProduct}` correspond to the values from the `lsusb` ID.
5. The rules file needs to also contain the ST-Link firmware updater ID, which is the easiest to obtain in the later steps (while trying to upload code):
   ```
   # STM32 ST-LINK/V2.1 on STM32F767ZIT6U dev board (firmware update mode)
   ACTION!="remove", SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="3748", MODE="0660", TAG+="uaccess"
   ```
6. Save the rules file (requires `sudo`).

## Connecting ST-Link to MCU


## Building and Flashing the Project


## Additional Troubleshooting
