parallella-bin
==============

This repository serves as an easily accessible repository of binary boot
images. The following table details the significance of each relase. To use
these images, just extract the archive and copy the files as is to the BOOT
partion of an SD card.
  

rel.14.1.24.tar.gz	-	Linux kernel 3.12  
                                Supports USB camera  



rel.13.11.25.tar.gz	-	Version shipped from 12/2013 to 1/2014  
                                6 month older version of kernel  
                                Based on ADI linux tree with HDMI  
                                No USB camera  

##########################################################################
How to create the FPGA bitstream? (parallella.bit.bin)

##########################################################################
How to create the Zynq first stage loader? (fsbl.elf)

##########################################################################
How to compile the ADI based linux kernel? (uImage)
git clone https://github.com/parallella/parallella-linux-adi
cd parallella-linux-adi
bash
export ARCH=arm
export CROSS_COMPILE=<your-toolchain-prefix> #eg arm-xilinx-linux-gnuabi-
export PATH=<path/to/your/arm-toolchain>:$PATH 
make ARCH=arm parallella_defconfig
make ARCH=arm uImage

###########################################################################
How to compile the device tree? (devicetree.dtb)
scripts/dtc/dtc -I dts -O dtb -o arch/arm/boot/zynq-zed.dtb arch/arm/boot/dts/zynq-zed.dts

###############################################################
How to create the qspi flash image? (parallella.bin)

1. Create a ".bif" configuration file with the following content:

the_ROM_image:
{
[bootloader]/path/to/fsbl.elf
 /path/to/u-boot.elf
}

2. Run bootgen:

  bootgen -image /path/to/image.bif -o i parallella.bin

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