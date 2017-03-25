All rights are of their respective owners.
It has been copied for archive purposes.

# ODROID-X

The ODROID-X is a very low cost, high performance development platform based on an Exynos 4412 ARM Cortex-A9 Quad Core 1.4GHz CPU. It has 6 USB 2.0 ports, micro HDMI, The board has several accessories available through Hardkernel such as 10.1" and 14" LVDS LCDs (with adapters), Wifi and Bluetooth adapters, 1.8v serial adapter (recommended if you're going to be any debugging...actually, it's recommended anyway so you won't have to pay shipping twice when you realize you need it later), and an eMMC storage module. They also have a camera module, but currently it does not work with Linux.

Other features of the board include:

*   ARMv7 Cortex-A9 Architecture
*   Samsung Exynos 4412 1.4GHz Processor
*   1GB RAM
*   Mali-400 Quad Core Graphics accelerator
*   1080p/720p video over HDMI, 1360x768 over LVDS
*   3.5mm headphone/microphone jacks
*   6x USB 2.0 Host ports
*   1x USB 2.0 Device port
*   50 pin I/O expansion port for LCD/I2C/UART/SPI/ADC/GPIO interfaces
*   Full size SDHC
*   eMMC module
*   Ethernet 10/100
*   MIPI-CAM camera connector (not yet supported)
*   90 mm x 94 mm base board size

## Links

* [Odroid Forum](http://forum.odroid.com/viewforum.php?f=130)
* [Hardkernel downloads](http://com.odroid.com/sigong/nf_file_board/nfile_board.php?tag=ODROID-X)
* [Odroid project hosting](http://oph.mdrjr.net/meveric/)
* [How to install Ubuntu in EMMC](http://com.odroid.com/sigong/prev_forum/t2440-guide-installing-the-latest-ubuntu-on-odroid-x-emmc.html)


## Images and Distributions

* [Arch Linux](http://nl2.mirror.archlinuxarm.org/os/)
* [Ubuntu 16.04 LTS](http://odroid.in/ubuntu_16.04lts/)
* [Odroid game station with XMBC](http://forum.odroid.com/viewtopic.php?f=11&t=2684)

## Arch Linux

The following text has been replicated from [Arch Linux documentation](https://archlinuxarm.org/platforms/armv7/samsung/odroid-x).

### SD Card Creation

Replace **sdX** in the following instructions with the device name for the SD card as it appears on your computer.

1.  Zero the beginning of the SD card:

    <pre>dd if=/dev/zero of=/dev/sdX bs=1M count=8</pre>

2.  Start fdisk to partition the SD card:

    <pre>fdisk /dev/sdX</pre>

3.  At the fdisk prompt, create the new partitions:
    1.  Type **o**. This will clear out any partitions on the drive.
    2.  Type **p** to list partitions. There should be no partitions left.
    3.  Type **n**, then **p** for primary, **1** for the first partition on the drive, and **enter** twice to accept the default starting and ending sectors.
    4.  Write the partition table and exit by typing **w**.
4.  Create the ext4 filesystem:

    <pre>mkfs.ext4 /dev/sdX1</pre>

5.  Mount the filesystem:

    <pre>mkdir root
    mount /dev/sdX1 root</pre>

6.  Download and extract the root filesystem (as root, not via sudo):

    <pre>wget http://os.archlinuxarm.org/os/ArchLinuxARM-odroid-x-latest.tar.gz
    bsdtar -xpf ArchLinuxARM-odroid-x-latest.tar.gz -C root</pre>

7.  Flash the bootloader files:

    <pre>cd root/boot
    ./sd_fusing.sh /dev/sdX
    cd ../..</pre>

8.  Unmount the partition:

    <pre>umount root</pre>

9.  Set the boot switches on the ODROID-X board to boot from SD:
    1.  Locate the jumper between the SD slot, USB ports, and heatsink, labeled for eMMC and SD selection
    2.  Place the jumper over just one of the pins (so you don't lose it), not over both pins.
10.  Insert the SD card into the board, connect ethernet, and apply 5V power.
11.  Use the serial console (with a null-modem adapter if needed) or SSH to the IP address given to the board by your router.
    *   Login as the default user _alarm_ with the password _alarm_.
    *   The default root password is _root_.

### eMMC Module Creation

In case you do not have the emmc to SD adapter, just repeat the procedure above when running from SD.

Pay attention to target the /dev/mmcblkX and /dev/mmcblkXp1, where X doesn't correspond to what is shown in 

```
df -h
# or
mount -l | grep -i mmc
```

4.  Power off the board: _poweroff_
5.  Remove the micro SD adapter, detach the eMMC module, and connect the eMMC module to its connector on the board.
6.  Set the boot switches on the ODROID-X to boot from eMMC:
    1.  Locate the jumper between the SD slot, USB ports, and heatsink, labeled for eMMC and SD selection
    2.  Place the jumper over both pins.
7.  Re-apply power the board.
8.  Use the serial console (with a null-modem adapter if needed) or SSH to the IP address given to the board by your router.
    *   Login as the default user _alarm_ with the password _alarm_.
    *   The default root password is _root_.

### Notes

*   The kernel will always recognize the SD card first if it is connected. The jumper selection only controls what appears first for U-Boot, and which device will be looked at for finding the bootloader.
*   To use the SD as additional storage while booting from eMMC:
    1.  Boot with only the eMMC module as described above.
    2.  Open _/boot/uEnv.txt_ in your favorite editor.
    3.  Uncomment the _mmcroot_ line, change _/dev/mmcblk0p1_ to _/dev/mmcblk1p1_, and save the file.
    4.  Power off the board, connect the SD card, and boot the board again. The SD card will be available under _/dev/mmcblk0_ now.
