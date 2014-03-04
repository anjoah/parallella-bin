parallella-bin
==============

This repository serves as an easily accessible repository of binary boot
images. The following table details the significance of each relase. To use
these images, just extract the archive and copy the files as is to the BOOT
partion of an SD card.
  
rel.14.02.06.tgz        -       Added support for Wifi
                                Added support for ataoe

rel.14.01.24.tgz	-	Migrated to Linux kernel 3.12  
                                Added support for USB camera  

rel.13.11.25.tgz	-	Version shipped from 12/2013 to 1/2014  
                                6 month older version of kernel  
                                Based on ADI linux tree with HDMI  
                                No USB camera  

##########################################################################  

How to compile the ADI based linux kernel? (uImage)  
git clone https://github.com/parallella/parallella-linux-adi  
cd parallella-linux-adi  
bash  
export ARCH=arm  
export CROSS_COMPILE=<your-toolchain-prefix> #eg arm-xilinx-linux-gnuabi-  
export PATH=<path/to/your/arm-toolchain>:$PATH  
make ARCH=arm parallella_defconfig  
make ARCH=arm LOADADDR=0x8000 uImage
###########################################################################  

How to compile the device tree? (devicetree.dtb)  
scripts/dtc/dtc -I dts -O dtb -o arch/arm/boot/zynq-zed.dtb arch/arm/boot/dts/zynq-zed.dts  
  
#########################################################################   

How to create the FPGA bitstream? (parallella.bit.bin)  
Use the Xilinx tool chain (ISE 14.4)   
  
##########################################################################  

How to create the Zynq first stage loader? (fsbl.elf)?  
Use the Xilinx tool chain (ISE 14.4)   

###############################################################  
How to compile u-boot? (u-boot.elf)  

  sudo apt-get install u-boot-tools  

  git clone https://github.com/parallella/parallella-uboot.git  

  cd parallella-uboot  

  bash  

  export ARCH=arm  

  export CROSS_COMPILE=<your-toolchain-prefix> #eg arm-xilinx-linux-gnuabi-  

  export PATH=<path/to/your/arm-toolchain>:$PATH   

  make parallella_config  

  make -j 2  
###################################################################  
How to create the qspi flash image? (parallella.bin)  

1. Create a ".bif" configuration file with the following content:  

the_ROM_image:  
{  
[bootloader]/path/to/fsbl.elf  
 /path/to/u-boot.elf  
}  

2. Run bootgen:  

  bootgen -image /path/to/image.bif -o i parallella.bin  
###################################################################  
How to create an SD card?

Method #1:
Download

Method #2:
1.Use gparted to crete two partitions:
  -The first partition will hold the boot loader, devicetree and kernel images. 
  It should be about 50MiB in size, and formatted with FAT file system. 
  Label this partition as "BOOT"
  -The second partition will occupy the rest of the available space. 
  It will store the system’s root file sytem, and should be formatted 
  with ext4 file system. This partition should be labelled as "rootfs"

2. Get a Linux/FPGA achive from http://github.com/parallella/parallella-bin.
   
3. Unzip the archive and copy files to BOOT partition on SD card

4. Download root file system from:
   http://releases.linaro.org/13.12/ubuntu/saucy-images/developer/linaro-saucy-server-20131216-586.tar.gz  

5. sudo tar -zxvf linaro-saucy-developer-20131216-586.tar.gz

6. cd binary

7. sudo rsync -a --progress ./ /media/"username"/rootfs

wget http://releases.linaro.org/14.01/ubuntu/saucy-images/nano/linaro-saucy-nano-20140126-627.tar.gz 
https://raw.github.com/parallella/parallella-bin/master/rel.14.02.06.tgz
sudo tar --strip-components=1 -C /media/aolofsson/rootfs -xzpf linaro-saucy-nano-20140126-627.tar.gz
sudo emacs sudo emacs /media/aolofsson/rootfs/etc/network/interfaces
;;insert following
auto lo
iface lo inet loopback
allow-hotplug eth0
auto eth0
iface eth0 inet dhcp


   
###################################################################  


