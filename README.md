```
+-------------------------------------------------+
|               [ BL1 - BootROM ]                 |
|    (Burned into hardware, immutable code)       |
|   - Detects boot medium (SD/eMMC/NAND)            |
|   - Loads BL2                                   |
+------------------------|------------------------+
                         v
+-------------------------------------------------+
|             [ BL2 - Preloader ]                 |
|   - Initializes CPU, RAM, UART, etc.            |
|   - Locates and loads the file                  |
|     xxBL31-UBOOT.FIP                            |
|   - Hands control to the FIP file               |
+------------------------|------------------------+
                         v
+-------------------------------------------------+
|             [ BL3 - FIP File ]                  |
|   Contains:                                     |
|     - BL31 (ATF – Arm Trusted Firmware)         |
|     - BL33 (uBoot – Primary Bootloader)         |
|                                                 |
|   Flow: BL2 -> BL31 -> BL33                     |
+------------------------|------------------------+
                         v
+-------------------------------------------------+
|          [ BL33 - uBoot Bootloader ]            |
|   - Locates the kernel (OpenWRT/Ubuntu/etc.)      |
|   - Loads the operating system image            |
|   - Jumps to the kernel's start                  |
+------------------------|------------------------+
                         v
+-------------------------------------------------+
|         [ OpenWRT / Operating System ]          |
|   - Kernel executes init                        |
|   - System becomes operational                  |
+-------------------------------------------------+
```
