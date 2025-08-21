# Untested, don't do this.

## Flashing the hearth 

1. Download the RK tooling
RKDevTool v2.86 https://dl.radxa.com/tools/windows/RKDevTool_Release_v2.86.zip
DriverAssistant v5.0 https://dl.radxa.com/tools/windows/DriverAssitant_v5.0.zip

2. Extract each zip

3. Update RKDevTool's config.ini to `Selected=2` under Language (if you want english)

4. Launch the RKDevTool, you should see a message at the bottom indicating no devices found

5. With the hearth powered off, plug in your micro usb cable. Hold down the Power and Vol Up buttons while plugging the power back in. The screen will remain black. You'll see the message on RKDevTool change to "Found One LOADER Device"

6. Set the loader to idbloader.img, and the uboot to u-boot-dtb.img

7. ...


===
sudo apt-get install gcc-aarch64-linux-gnu rkdeveloptool

git clone https://github.com/rockchip-linux/u-boot.git u-boot-rockchip

make rk3568-spl-spi-nand_defconfig
make ARCH=arm CROSS_COMPILE=aarch64-linux-gnu- KCFLAGS="-Wno-error"

./tools/mkimage -n rk3568 -T rksd -d tpl/u-boot-tpl.bin:spl/u-boot-spl.bin idbloader.img

# if no .itb file was made
 make ARCH=arm CROSS_COMPILE=aarch64-linux-gnu- u-boot.itb V=1 KCFLAGS="-Wno-error"                                  
