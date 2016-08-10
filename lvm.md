* How to use lvm in Fedaro.

$ sudo yum install lvm2
Load the necessary module(s) as root:

$ sudo modprobe dm-mod
Scan your system for LVM volumes and identify in the output the volume group name that has your Fedora volume (mine proved to be VolGroup00):

$ sudo vgscan
Activate the volume:

$ sudo vgchange -ay VolGroup00
Find the logical volume that has your Fedora root filesystem (mine proved to be LogVol00):

$ sudo lvs
Create a mount point for that volume:

$ sudo mkdir /mnt/fcroot
Mount it:

$ sudo mount /dev/VolGroup00/LogVol00 /mnt/fcroot -o ro,user
