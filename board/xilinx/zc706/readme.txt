This is the Buildroot board support for the Xilinx ZC706. The ZC706 is
a development board based on the Xilinx Zynq-7000 based
All-Programmable System-On-Chip.

ZC706 information including schematics, reference designs, and manuals
are available from
http://www.xilinx.com/products/boards-and-kits/ek-z7-zc706-g.html.

uboot.bin  -- U-Boot SPL w/ Xilinx boot.bin wrapper
---------------------------------------------------

Due to licensing issues, the files ps7_init.c/h are not distributed
with the U-Boot source code.  These files are required to make a
boot.bin file.

If you already have the Xilinx tools installed, the following sequence
will unpack, patch and build the rfs, kernel, uboot, and uboot-spl.

make zynq_zc706_defconfig
make uboot-patch
cp ${XILINX_SDK_LIB}/hwplatform_templates/ZC706_hw_platform/ps7_init.{c,h} \
   output/build/uboot-xilinx-v2014.1/board/xilinx/zynq/

Where ${XILINX_SDK_LIB} is ${XILINX}/SDK/${VERSION}/data/embeddedsw/lib.

After copying these files into the U-Boot source tree, you can
continue the build with:

make

*Notice*
While the build will successfully complete without the ps7_init.*
files, the uboot.bin file generated by this configuration will not
function properly on the ZC706. Therefore, it is imperative that the
ps7_init.* files be copied into the U-Boot source tree any time the
clean, or uboot-dirclean targets are executed.

Resulting system
----------------

A FAT32 partition should be created at the beggining of the SD Card
and the following files should be installed:

- boot.bin
- devicetree.dtb
- uImage
- uramdisk.image.gz
- u-boot.img

All needed files can be taken from <output>/images/

cp <output>/images/boot.bin /media/sdcard/
cp <output>/images/uImage /media/sdcard/
cp <output>/images/u-boot.img /media/sdcard/
cp <output>/images/zynq-zc706.dtb /media/sdcard/devicetree.dtb
cp <output>/images/rootfs.cpio.uboot /media/sdcard/uramdisk.image.gz
