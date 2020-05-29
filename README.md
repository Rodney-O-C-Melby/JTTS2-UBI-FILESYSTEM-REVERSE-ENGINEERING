# JTTS2-UBI-FILESYSTEM-REVERSE-ENGINEERING
Reverse Engineering a JFTTS2/UBI Filesystem from .bin file to files and folders.

REVERSE ENGINEERING JFFS2 UBI FILESYSTEM (TP-LINK ROUTER) - for education only

DEPENDANCIES:

ARCH AUR NAME\t       PUBLIC REPOSITORIES FOR ANY SYSTEM</br>
binwalk\t             https://github.com/ReFirmLabs/binwalk</br>
ubi_reader/t          https://github.com/jrspruitt/ubi_reader</br>
jefferson-git/t       https://github.com/sviehb/jefferson</br>
Linux kernel/t        https://www.kernel.org/</br>

# Extract Filesystem
Extracts a bin file to a filesystem (-e=extract, -M=recursivly, <FILENAME>=Input .bin file)</br>
binwalk -e -M <FILENAME></br>

# Get UBI/MTD info with mtd-utils
Handy tool mtd-utils which includes:</br>
    ubinfo - provides information about UBI devices and volumes found in the system;</br>
    ubiattach - attaches MTD devices (which describe raw flash) with UBI which creates corresponding UBI devices;</br>
    ubidetach - detaches MTD devices from UBI devices (the opposite to what ubiattach does);</br>
    ubimkvol - creates UBI volumes on UBI devices;</br>
    ubirmvol - removes UBI volumes from UBI devices;</br>
    ubiblock - manages block interfaces for UBI volumes. See here for more information;</br>
    ubiupdatevol - updates UBI volumes; this tool uses the UBI volume update feature which leaves the volume in "corrupted" state if the update was interrupted; additionally, this tool may be used to wipe out UBI volumes;</br>
    ubicrc32 - calculates CRC-32 checksum of a file with the same initial seed as UBI would use;</br>
    ubinize - generates UBI images;</br>
    ubiformat - formats empty flash, erases flash and preserves erase counters, flashes UBI images to MTD devices;</br>
    mtdinfo - reports information about MTD devices found in the system.
    
REF: http://www.linux-mtd.infradead.org/doc/ubi.html


# Create a UBI Filesystem 
(Always needs to be formatted to avoid corruption)</br>
sudo modprobe nandsim first_id_byte=0x2c second_id_byte=0xda third_id_byte=0x90 fourth_id_byte=0x95</br>
sudo flash_erase /dev/mtd0 0 0</br>
sudo ubiformat /dev/mtd0 -s 2048 -O 2048</br>
sudo modprobe ubi</br>
sudo ubiattach -m 0 -d 0 -O 2048</br>
sudo ubimkvol /dev/ubi0 -N RFS -s $((0xA00000))</br>
sudo ubiupdatevol /dev/ubi0_0 RFS.bin</br>
sudo mount /dev/ubi0_0 /mnt
