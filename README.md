To check out PULPino on a ZedBoard, you can use pre-built linux sd card image, see
zedboard-pulp-boot.tar.gz and zedboard-pulp-root.tar.gz.

The default username is: root
The default password is: pulp


To run a simple blinken lights application, you can use

    /root/spiload /root/led_demo.spi



The guide below roughly follows the instructions on the [xilinx
wiki](http://www.wiki.xilinx.com/Prepare+Boot+Medium) on how to setup an sd
card for a ZedBoard.

1. Partition the sd card

   Open fdisk for you sd card device

   ```
   fdisk /dev/sdX
   ```

   Create a new partition table, type `o`

   Now create the partitions

   ```
   Command (m for help): n
   Partition type:
    p primary (0 primary, 0 extended, 4 free)
    e extended
   Select (default p): p
   Partition number (1-4, default 1): 1
   First sector (2048-15759359, default 2048):
   Using default value 2048
   Last sector, +sectors or +size{K,M,G} (2048-15759359, default 15759359): +200M

   Command (m for help): n
   Partition type:
    p primary (1 primary, 0 extended, 3 free)
    e extended
   Select (default p): p
   Partition number (1-4, default 2): 2
   First sector (411648-15759359, default 411648):
   Using default value 411648
   Last sector, +sectors or +size{K,M,G} (411648-15759359, default 15759359):
   Using default value 15759359
   ```


   Now make the partitions bootable

   ```
   Command (m for help): a
   Partition number (1-4): 1

   Command (m for help): t
   Partition number (1-4): 1
   Hex code (type L to list codes): c
   Changed system type of partition 1 to c (W95 FAT32 (LBA))

   Command (m for help): t
   Partition number (1-4): 2
   Hex code (type L to list codes): 83
   ```


   Write the new partition table by typing `w`


2. Create new file systems on the new partitions

   ```
   mkfs.vfat -F 32 -n boot /dev/sdX1
   mkfs.ext4 -L root       /dev/sdX2
   ```


3. Mount the boot partition and put the boot images on there

   ```
   mkdir /tmp/boot
   mount /dev/sdX1 /tmp/boot
   cd /tmp/boot
   tar -xzvf ~/Downloads/zedboard-pulp-boot.tar.gz
   ```

4. Mount the root partition and copy the contents from the tar gz file on there

   ```
   mkdir /tmp/root
   mount /dev/sdX1 /tmp/root
   cd /tmp/root
   tar -xzvf ~/Downloads/zedboard-pulp-root.tar.gz
   ```

5. Unmount everything and put the sd card into your zedboard

   ```
   umount /tmp/boot
   umount /tmp/root
   ```
