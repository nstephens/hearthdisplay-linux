# UNTESTED

Do not attempt to use this with any expectations of it working. It has not yet been tested. 

But when it does work...

## Install Instructions
### Step 1: Identify your SD card
```
# List all block devices
lsblk

# Or use fdisk
sudo fdisk -l
```
### Step 2: Unmount the SD card if mounted
```
# Replace sdX with your actual device
sudo umount /dev/sdX*
```

### Step 3: Create the bootable SD card
For RK3568, you need to write the bootloader at a specific offset and create partitions:

```
# Replace /dev/sdX with your SD card device (e.g., /dev/sdb)
export SDCARD=/dev/sdX

# Write U-Boot bootloader at sector 64 (32KB offset)
sudo dd if=output/images/u-boot.bin of=$SDCARD seek=64 conv=notrunc bs=512

# Create partition table and partitions
sudo parted $SDCARD --script -- mklabel msdos \
  mkpart primary fat32 1MiB 65MiB \
  mkpart primary ext4 65MiB 100% \
  set 1 boot on

# Format the partitions
sudo mkfs.vfat ${SDCARD}1
sudo mkfs.ext4 ${SDCARD}2

# Mount and copy files
mkdir -p /tmp/sd-boot /tmp/sd-root
sudo mount ${SDCARD}1 /tmp/sd-boot
sudo mount ${SDCARD}2 /tmp/sd-root

# Copy kernel and device tree to boot partition
sudo cp Image rk3568-evb1-v10.dtb /tmp/sd-boot/ 

# Extract and copy root filesystem
sudo dd if=rootfs.ext2 of=${SDCARD}2 bs=1M

# Create boot.txt for U-Boot
sudo tee /tmp/sd-boot/boot.txt << EOF
setenv bootargs console=ttyS2,1500000n8 root=/dev/mmcblk0p2 rootfstype=ext4 rootwait rw
load mmc 0:1 \${kernel_addr_r} Image
load mmc 0:1 \${fdt_addr_r} rk3568-evb1-v10.dtb
booti \${kernel_addr_r} - \${fdt_addr_r}
EOF

# Convert boot.txt to boot.scr (U-Boot script)
sudo apt install u-boot-tools
sudo mkimage -A arm64 -O linux -T script -C none -d /tmp/sd-boot/boot.txt /tmp/sd-boot/boot.scr

# Unmount
sudo umount /tmp/sd-boot /tmp/sd-root
sudo rmdir /tmp/sd-boot /tmp/sd-root
```
