12/1/15

Goal: To add sound support, USB C-920 camera support, openCV, gnuplot, gsl gnuradio, and 
vlc dependencies to the zedboard.  Modifing the 3.12+gitAUTOINC+4557b7c2d2-r0 Linux Kernel.  
 
Requirements: 
	Xilinx 14.6 & Vivado 2013.4  used by meta-topic to create fpga-bin.
	meta-topic from "https://github.com/topic-embedded-products/meta-topic.git" 
        Disk /dev/sdb: 3963 MB, 3963617280 bytes

   	Device Boot      Start         End      Blocks   Id  System
	/dev/sdb1            2048       34815       16384   83  Linux
	/dev/sdb2           34816     7741439     3853312   83  Linux

        16M partition formatted vfat w/cmd "mkfs -t vfat -n ZED_BOOT /dev/sdb1"
        ext3 formatted w/cmd "mkfs -t ext4 -L ROOT_FS /dev/sdb2"
 
Using the poky dora c3cccbea7abd4af3d603b2be42b0c7291ff21946 branch and meta-topic branch 63a952c04ba10b6f40edf635413181e2104f51cf patched with the file "dora_meta-topic-diff_63a952c04ba10b6f40.patch".
Need the required meta data
poky dora:c3cccbea7abd4af3d603b2be42b0c7291ff21946
	meta-topic master:63a952c04ba10b6f40edf635413181e2104f51cf
	meta-oe/meta-oe dora:191499a2b54d04855284347ce5a067f998646be4
	meta-oe/meta-multimedia
	meta-oe/meta-gnome

cd POKY/dora_build/poky/
. oe-init-build-env
cp ~/wkg/zedboard_yocto/wkg_conf_files/local.conf conf/
cp ~/wkg/zedboard_yocto/wkg_conf_files/bblayers.conf conf/

bitbake core-image-sato

The patch is first tested following the command "git checkout 63a952c04ba10b6f40edf635413181e2104f51cf"
in top level of meta-topic.  The patch is tested with the command "patch -p1 --dry-run < ~/wkg/zedboard_yocto/dora_meta-topic-diff_63a952c04ba10b6f40.patch"
should indicate that the following files will be patched.
	patching file recipes-core/busybox/busybox_1.2%.bbappend
	patching file recipes-kernel/linux-zynq/linux-topic/zedboard/defconfig
	patching file recipes-multimedia/alsa/alsa-utils_1.%.bbappend
The patch can then be applied with the command "patch -p1  < ~/wkg/zedboard_yocto/dora_meta-topic-diff_63a952c04ba10b6f40.patch".

The initial image provides the core-image-sato plus the packages found in 
variable IMAGE_INSTALL_append found in the file local.conf.

After creating 2 partitions on the SD card


First Partion 
eb6774fd577658290b22b7b03a26127029a1aa72  autorun.scr
39a79185b12d5fb15eff84ea61b5e1772ccd6179  BOOT.bin
53650fabee2a651018ee09dcef8a4e74eee4242d  devicetree.dtb uImage-zynq-zed-adv7511.dtb -> uImage--3.12+git0+4557b7c2d2-r0-zynq-zed-adv7511-20140329190232.dtb
1d9ef4b7f3782e2e3bcb6b48619a6dd663b5d571  uImage uImage -> uImage--3.12+git0+4557b7c2d2-r0-zedboard-20140329190232.bin

 
2nd Partition 
e9d2b4dd218037ea52b48f7c1dd27ebc291838ad  core-image-sato-zedboard.tar.gz
core-image-sato-zedboard.tar.gz -> core-image-sato-zedboard-20151127000825.rootfs.tar.gz

/dev/sdb2        3727232   1015736   2502448  29% /media/ROOT_FS
/dev/sdb1          16334      7362      8972  46% /media/ZED_BOOT

Linux zedboard 3.12.0 #1 SMP PREEMPT Sat Mar 29 13:42:20 MDT 2014 armv7l GNU/Linux
rpm -qa | sort > zedbrd_pkgs.txt
aplay speech_dft.wav 
Playing WAVE 'speech_dft.wav' : Signed 16 bit Little Endian, Rate 22050 Hz, Mono
More testing is found in the file doc/Zedboard_Yocto_test.pdf of this repository.

