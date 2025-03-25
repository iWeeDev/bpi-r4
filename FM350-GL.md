# FM350-GL and BE14 finally works on BPI-R4

**Author:** OtakuNekoP (Leo Xu)  
**Date:** October 5, 2024, 11:16am

Since the BE14 233 fw and patch had been pushed to OpenWrt last week, after some time of fiddling around I managed to get the FM350-GL and BE14 working on the BPI-R4.

Here’s a summary of some of the operating points in case anyone needs them, and please point out any mistakes, thanks.

---

## Compilation Steps

### 1. Hardware Setup

- **Insert the FM350-GL** into the PCI-E 2 slot labeled **SIM1**.
- **Attach** the appropriate antenna.
- **Install** a larger heatsink to prevent overheating.

### 2. Prepare the Compilation Environment

- Use **Ubuntu 24.04.3 Server**.
- Follow OpenWrt’s toolchain installation guide: [OpenWrt Toolchain](https://openwrt.org).

### 3. Pull Source Code

- Follow the OpenWrt build system guide: [OpenWrt Build System](https://openwrt.org).

### 4. Configure Build Options

Run `make menuconfig` and select the necessary kernel and LuCI modules:

- `mt7986-wo-firmware`
- `mt7988-2p5g-phy-firmware`
- `kmod-mtk-t7xx`, `kmod-phy-aquantia`, `kmod-sfp`, `kmod-wwan`
- `kmod-usb-net-rndis`, `kmod-usb-serial`, `kmod-usb-serial-option`, `kmod-usb3`
- `kmod-mt7915e`, `kmod-mt7986-firmware`, `kmod-mt7996-233-firmware`, `kmod-mt7996e`
- `luci-proto-mbim`, `luci-proto-modemmanager`, `luci-proto-ncm`
- `comgt`, `pciutils`, `usbutils`

### 5. Compile the Firmware

- **Start the compilation:**

  ```bash
  make -j$(nproc) defconfig download
  ```

- **Modify the Device Tree Source (DTS) file** to disable PCI-E mode and switch to USB3 Mode:
  - **Path:**  
    `./build_dir/target-aarch64_cortex-a53_musl/linux-mediatek_filogic/linux-6.6.52/arch/arm64/boot/dts/mediatek/mt7988a-bananapi-bpi-r4.dts`
  - **Change line 290:**  
    from  
    ```dts
    status = "okay";
    ```  
    to  
    ```dts
    status = "disabled";
    ```

- **Compile with:**

  ```bash
  make -j$(nproc) world
  ```

- **Flash** the newly compiled firmware onto the BPI-R4.
- **Power off and reboot** the device.

### 6. Install the ATC Package for Better Management

- Download and install the ATC packages:

  ```bash
  wget https://github.com/mrhaav/openwrt/raw/master/atc/fib-fm350_gl/atc-fib-fm350_gl_2024-08-04-0.2_all.ipk
  opkg install atc-fib-fm350_gl_2024-08-04-0.2_all.ipk
  wget https://github.com/mrhaav/openwrt/raw/master/atc/luci-proto-atc_20230813-0.2_all.ipk
  opkg install luci-proto-atc_20230813-0.2_all.ipk
  ```

- **Create a new ATC proto port** in the interface tab and configure the APN.
- **Identify** the modem device (e.g., `/dev/ttyUSB4`).

By following these steps, you can set up a Wi-Fi 7 AP with a 5G WWAN connection on the Banana Pi BPI-R4.

See my blog for more detailed steps and lengthy experimental procedures and references: [Blog](https://blog.nyamoe.com/2024/10/using-the-fibocom-fm350-gl-5g-module-on-banana-pi-bpi-r4/)

---
[Full post](https://forum.banana-pi.org/t/fm350-gl-and-be14-finally-works-on-bpi-r4/19170)


