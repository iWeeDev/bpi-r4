for SD card write this (starting from SD card)
´´´
dd if=bl2sd.img of=/dev/mmcblk0p1
´´´

for eMMC write this (starting from NAND)

´´´
dd if=bl2emmc.img of=/dev/mmcblk0boot0
dd if=openwrt-mediatek-filogic-bananapi_bpi-r4-sdcard.img of=/dev/mmcblk0 (or antoher image)
mmc bootpart enable 1 1 /dev/mmcblk0
´´´
