## Create fs.img <uname -r> == linux-6.1.11
```
mkinitramfs -k linux-6.1.11 -o fs.img

```
## Run qemu with the compiled linux kernel
```
qemu-system-x86_64 -kernel ~/linux-6.1.11/arch/x86_64/boot/bzImage -initrd initrd.img
```
## Here are the steps to create a ramfs.img file in WSL (Windows Subsystem for Linux) to run the Linux kernel in QEMU (Quick EMUlator):
## Install QEMU in WSL:
```
sudo apt-get update
sudo apt-get install qemu-system-x86 qemu-utils
```
## Create a directory for the ramfs image:
```
mkdir ~/ramfs
```
## Populate the directory with the files you want to include in the ramfs image:
```
cd ~/ramfs
```
## Create any necessary directories
```
mkdir -p bin etc dev lib proc sys tmp
```
## Copy files to the directories as needed
##Create the ramfs.img file:
```
cd ~
dd if=/dev/zero of=ramfs.img bs=1M count=64
mkfs.ext2 ramfs.img
``` 
## Mount the ramfs.img file to the ramfs directory:
```
sudo mount -o loop ramfs.img ~/ramfs
```
## Copy the contents of the ramfs directory to the ramfs.img file:
```
sudo cp -r ~/ramfs/* /mnt
```
## Unmount the ramfs.img file:
```
sudo umount ~/ramfs
```
## Run the Linux kernel in QEMU with the ramfs.img file as the root file system:
```
qemu-system-x86_64 -kernel /path/to/linux-kernel -initrd ramfs.img -append "root=/dev/ram rw"
```
## Create a virtual disk image: You can create a virtual disk image using the qemu-img command, for example:
```
qemu-img create -f qcow2 rootfs.img 2G
```
``` Boot the virtual machine with the virtual disk image: You can use the -hda option of the 
 QEMU command to specify the virtual disk image created in step 1 as the virtual hard disk of the virtual machine, for example:
```
```
qemu-system-x86_64 -hda rootfs.img -cdrom /path/to/os-installation-image.iso
```
```
In this command, the -cdrom option is used to specify the ISO image of the operating system installation image.

Perform the operating system installation: Once the virtual machine is running, you can perform the operating system installation in the usual way, using the ISO image as the installation source.

Boot the virtual machine using the virtual disk image: After the installation is complete, you can shut down the virtual machine and then boot it again using the 
```
## virtual disk image as the root file system, for example:
```
qemu-system-x86_64 -hda rootfs.img
```

