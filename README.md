# OpenWrt-Baidu-M-100

OpenWrt trunk patch for Baidu M-100

## Compile
	
	mkdir openwrt
	cd openwrt
	git clone git://git.openwrt.org/openwrt.git trunk
	git clone https://github.com/JiapengLi/OpenWrt-Baidu-M-100.git
	cd trunk
	patch -p1 < ../OpenWrt-Baidu-M-100/openwrt-add-support-for-baidu-m-100.patch
	./scripts/feeds/update -a
	./scripts/feeds/install -a

	rm -rf .config tmp
	make menuconfig     // choose the right board and 
                      // Target System: Ralink RT288x/RT3xxx
                      // Subtarget: MT7620a based boards
                      // Target Profile: BAIDU M-100

	make -j n           // n="core number"+1; for example 3 when dual core;

Wait wait wait...  
Sometimes the compilation needs several hours. When finish, the bin file is in `bin/ramips`.

## Backup
By now, OpenWrt doesn't support all features of mt7620a chip, so be careful with you M-100 router. Before installation, backup the firmware first. M-100 open the telnet service, so we could use telnet to log in.

	telnet 192.168.169.1     // For windows user, firewall may need to be closed

### Layout

Creating 7 MTD partitions on "raspi":  
0x000000000000-0x000000800000 : "ALL"  
0x000000000000-0x000000030000 : "Bootloader"  
0x000000030000-0x000000040000 : "Config"  
0x000000040000-0x000000050000 : "Factory"  
0x000000050000-0x0000001d5fb6 : "Kernel"  
0x0000001d5fb6-0x000000800000 : "RootFS"  

### Copy from M-100 to PC
	cat /dev/mtd0 > /tmp/bwmb-all.bin             // Backup Image, replace mtd0 with other partition
	tftp -p -l bwmb-all.bin 192.168.xxx.xxx       // Save Image to PC

## Installation

## Advanced
The whole process to hack M-100.

## End
