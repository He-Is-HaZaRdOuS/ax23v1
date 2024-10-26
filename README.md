# TP-Link Archer AX23 Emulation Procedure to Reach Failsafe Mode

This document outlines the steps to emulate the Archer AX23 firmware and reach failsafe mode using the Firmware Analysis Toolkit and other necessary tools.

## Prerequisites

1. **Download and Install Firmware Analysis Toolkit**
    First, clone the [Firmware Analysis Toolkit](https://github.com/attify/firmware-analysis-toolkit) repository:

    ```bash
    git clone https://github.com/attify/firmware-analysis-toolkit
    cd firmware-analysis-toolkit

Follow the installation instructions provided in the repository to set up the toolkit.

2. **Obtain the Firmware**
   Download the firmware for the Archer AX23 (Hardware revision 1) router from the official website [Archer AX23](https://www.tp-link.com/en/support/download/archer-ax23/v1/#Firmware).
   In this case, the firmware file is named `Archer AX23(EU)_V1_220216`.

## Emulation Procedure

1. **Generate Firmadyne Environment**
    Utilize the FAT utility provided in the Firmware Analysis Toolkit to generate a Firmadyne environment using the firmware file:

    ```bash
    ./fat.py <path_to_firmware_file>

Replace <path_to_firmware_file> with the path to your Archer AX23 firmware file.

2. **Update Kernel Parameter**
    Open the run.sh script located in the Firmadyne directory and locate the kernel parameter.
    Replace the existing kernel parameter with a newer kernel version that you can obtain from EMUX [Template Kernels](https://github.com/therealsaumil/emux/blob/master/files/emux/template/kernel/):
    In this case, the kernel file is named `vmlinux-3.18.109-malta-le`.

#### Example line to replace
  ${QEMU} -m 256 -M ${QEMU_MACHINE} -kernel ${KERNEL} \

##### Replace with
  ${QEMU} -m 256 -M ${QEMU_MACHINE} -kernel <path_to_new_kernel_file>

3.  **Mount the Filesystem Image for Repairs**
    Mount the filesystem image and repair it using the following command:

      ```bash
      sudo losetup -o 1048576 /dev/loop1 /home/<user>/firmadyne/scratch/<image_id>/image.raw
      sudo e2fsck -y /dev/loop1
      sudo losetup -d /dev/loop1

4. **Start the Emulation**
    Start the emulation using the following command:

    ```bash
    ./run.sh <image_id>

Watch out for the boot message to enter the failsafe mode.
Press `Ctrl + A` followed by `X` to exit the emulation.

Screenshot of the failsafe mode:
![Alt text](screenshots/failsafemode.png)

## Notes
- This procedure is based on the Archer AX23 firmware and may require modifications for other firmware versions.
- This procedure is intended for educational purposes only.
- The procedure is likely to work with other TP-Link routers as well. (Especially those with similar hardware and firmware structure like Hardware Revision 1.20 of Archer AX23)

References:
- [Firmware Analysis Toolkit](https://github.com/attify/firmware-analysis-toolkit)
- [Firmadyne](https://github.com/firmadyne/firmadyne)
- [EMUX (Formerly ARMX)](https://github.com/therealsaumil/emux)
- [TP-Link Archer AX23 Firmware](https://www.tp-link.com/us/support/download/archer-ax23/#Firmware)
```
