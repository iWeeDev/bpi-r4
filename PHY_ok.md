Configuring the **combined USB/PCIe PHY** is not typically something you do manually as an average user, as it is usually handled automatically by the operating system and the motherboard's **firmware**. However, on some boards like the **Banana Pi BPI-R4**, you may need to make some adjustments to the **software**, **firmware**, or **system configurations** for the **PHY** to work correctly with the device or port you are using.

Here is a general idea of how you could configure or ensure that the **PHY** works correctly:

### 1. **Ensure that the firmware is up to date**
- Often, **PHY** configurations are integrated into the device's firmware, which is the software that runs when the system powers up, before the operating system loads.
- It is important to ensure that the **firmware** is up to date, as updates can include improvements or fixes for managing ports like **USB** and **PCIe**.

To **update the firmware** of your board, consult the official documentation of **Banana Pi** or any firmware updates available in the official **OpenWrt** repository or the distribution you're using. Some boards allow firmware updates via **.bin** files or specific commands.

### 2. **Operating system configuration (OpenWrt or Linux)**
- In the operating system, such as **OpenWrt** or a **Linux** distribution, the **PHY** drivers need to be loaded correctly for **USB** or **PCIe** devices to work as expected.
- To verify that the **PHY** and ports are configured properly, you can use **dmesg** to see system messages that indicate if the **PHY** drivers were loaded correctly.

   You can use the following commands to check system messages related to **USB** or **PCIe**:
   ```bash
   dmesg | grep -i usb
   dmesg | grep -i pcie
   ```
   This will show information about the drivers and interfaces that are being loaded for those devices.

### 3. **Check the Device Tree (DTS) configuration**
- Boards like the **Banana Pi BPI-R4** often have **Device Tree (DTS)** configurations, which are files that tell the operating system how to configure various hardware components.
- In some cases, you may need to modify these files to specify whether you want to use a **USB** port or a **PCIe** port through the same connector or even activate specific features of the **PHY**.

For example, in **Linux** and **OpenWrt**, the **Device Tree** configuration is generally found in the configuration files inside **/boot/dtbs/** or in the kernel directory. To check if the **PHY** is configured correctly, you may need to edit these files, though this is advanced and requires a good understanding of the system.

### 4. **Check loaded modules and drivers**
- In some cases, **Linux** modules or **OpenWrt** drivers for **USB** and **PCIe** must be correctly installed for the **PHY** to work as expected.
- To check which modules are loaded, you can use the following commands:

   ```bash
   lsmod  # To view the loaded kernel modules.
   lspci  # To see connected PCIe devices.
   ```

   If your **USB** or **PCIe** device does not appear, you may need to load a specific module using the following command:

   ```bash
   modprobe <module-name>
   ```

### 5. **Configure the port via uBoot or boot configuration**
- Sometimes, the configuration of how the **M.2** port is used (USB or PCIe) is defined in the **bootloader** or in the boot configuration (like **uBoot**).
- If you have access to **uBoot**, you may need to configure the port parameters to use it correctly as **PCIe** or **USB**. This is more advanced and depends on the board's model and configuration.

### 6. **Verify the behavior on the device**
Once you've made all the configurations, you can verify if the device is being recognized correctly:

- For **PCIe**:
  ```bash
  lspci  # Shows connected PCIe devices.
  ```

- For **USB**:
  ```bash
  lsusb  # Shows connected USB devices.
  ```

If you see that the device or module is correctly recognized in the system, the **PHY** is likely functioning properly.

### Conclusion
In summary, configuring the **combined USB/PCIe PHY** is usually not something done manually, but it is important to ensure that the **firmware** is up to date, the drivers are installed, and the system configuration files (such as the **DTS**) are properly set. If everything is correct, the system should detect and manage the connected device without issues.
