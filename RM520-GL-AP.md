### Quectel Drivers Configuration on OpenWRT
I managed to get it working with Quectel drivers. After several hours of testing, I abandoned the purely OSS MBIM solution.

### Band Selection on the Modem
The first step was to ensure that the bands were correctly selected on the modem. Since I only want 5G or LTE, I configured the following:

```bash
./qmbimat -d /dev/wwan0mbim0 -a AT+QNWPREFCFG="mode_pref",NR5G:LTE
```

### Installed Packages
The packages I have installed are:

```bash
luci-proto-modemmanager
mbim-utils
modemmanager
modemmanager-rpcd
kmod-mhi-wwan-ctrl
kmod-mtk-t7xx
kmod-usb-acm
kmod-usb-atm
kmod-usb-atm-cxacru
kmod-usb-atm-speedtouch
kmod-usb-atm-ueagle
kmod-usb-net-kalmia
kmod-usb-net-qmi-wwan
kmod-usb-serial-option
kmod-usb-serial-wwan
kmod-mhi-wwan-ctrl
kmod-mhi-wwan-mbim
kmod-usb-net-qmi-wwan
kmod-usb-serial-wwan
kmod-wwan
wwan
kmod-qcom-qmi-helpers
kmod-usb-net-qmi-wwan
libqmi
luci-proto-qmi
qmi-utils
uqmi
kmod-mhi-bus
kmod-mhi-net
kmod-mhi-pci-generic
kmod-mhi-wwan-ctrl
kmod-mhi-wwan-mbim
kmod-qrtr-mhi
```

I'm not sure if all of them are necessary, but at least some of these are.

### Building the Kernel Module and Quectel Utility
First, the following OpenWRT version is used:

```bash
git clone git@github.com:openwrt/openwrt.git
cd openwrt
git checkout 6df0e3d # hash suffix from release build ID
wget https://downloads.openwrt.org/releases/24.10.0/targets/mediatek/filogic/config.buildinfo -O .config
wget https://downloads.openwrt.org/releases/24.10.0/targets/mediatek/filogic/feeds.buildinfo -O feeds.conf
./scripts/feeds update -a
./scripts/feeds install -a
```

Then, the Quectel driver and QConnectManager are compiled using the replicated kernel version (it must match exactly the kernel version being used). These packages can be obtained from Quectel (or possibly other sources, although I am unsure of the redistribution license):

```bash
unzip Quectel_Linux_PCIE_MHI_Driver_V1.3.7.zip -d package/kernel/
unzip Quectel_QConnectManager_Linux_V1.6.7.zip -d package/utils/
make -j $(($(nproc)+1)) toolchain/install
make -j $(($(nproc)+1)) target/linux/compile
make package/utils/quectel-CM-5G/{clean,compile} V=sc
make package/kernel/pcie_mhi/{clean,compile} V=sc
```

### Installation on the Development Board
Once built, it must be copied to the device and installed:

```bash
scp -O bin/targets/mediatek/filogic/packages/kmod-pcie_mhi_*.ipk user@router:/tmp
scp -O bin/packages/aarch64_cortex-a53/base/quectel-CM-5G_*.ipk user@router:/tmp
ssh user@router
cd /tmp; opkg install *.ipk
```

### Configuration and Testing
After installation, all existing ModemManager interfaces were disabled (as well as the service if running). The router and modem were powered off (using `poweroff` and physically removing the adapter to 'reset' them). After restarting, it was verified that the kernel module was loaded:

```bash
lsmod | grep pcie_mhi
# pcie_mhi              151552  0
```

A session was started (e.g., with `screen`) to run the service:

```bash
screen -S modem
quectel-CM
# (Detached with Ctrl-A D)
```

Finally, internet connectivity was tested using the new interface:

```bash
ping 1.1.1.1 -c 4 -I rmnet_mhi0.1
# PING 1.1.1.1 (1.1.1.1): 56 data bytes
# 64 bytes from 1.1.1.1: seq=0 ttl=57 time=201.485 ms
# 64 bytes from 1.1.1.1: seq=1 ttl=57 time=28.409 ms
# 64 bytes from 1.1.1.1: seq=2 ttl=57 time=45.255 ms
# 64 bytes from 1.1.1.1: seq=3 ttl=57 time=45.153 ms
#
# --- 1.1.1.1 ping statistics ---
# 4 packets transmitted, 4 packets received, 0% packet loss
# round-trip min/avg/max = 28.409/80.075/201.485 ms
```

### Automation
To facilitate usage, it is recommended to configure `quectel-CM` to start automatically at system boot, either via an init script or a systemd service.

### Additional Note (Debian)
As specific information for Debian, the guide "How to build out of tree Kernel Module on Debian · GitHub" may be useful for building a kernel module independently. Obviously, you can ignore the OpenWRT-related part and adapt the process to your own operating system.
