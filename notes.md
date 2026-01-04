# VIRTUAL MACHINE
WHats a virtual machine?
- no separate hardware needed
- you can install any other os on top of an existing os. 
- achieved through HYPERVISOR by sharing hardware resources
- isolated from host machine


hypervisor
host multiple virtual computers on a single computer
famous ones : virtualbox (OSS)
- hypervisor takses hardware res from host OS. 
- create virtual cpu, virtual ram, v storage, for each vmachnie

benefits
- experitment
- doesnt damage main os
- no new hardware needed
- test apps on diff platforms
- abstraction of OS from hardware
- secure


HYPERVISOR 
TYPE 2 : creating VMs on top of Host OS of the hardware
h/W -> host os -> hypervisor -> guest os
eg : virtualbox
for personal usage

TYPE 1 : creating VMs on top of hardware directly
AKA Bare metal hypervisor
h/W -> hypervisor -> guest os
eg: vmware esxi, ms hyper v
for servers and large usage
cloud platforms use
helps use all the resources of the hardware with any combo


vms provide vm image (vmi), which includes os and apps... so it can be backed up easily and helps to be separated from the hardware. 
you can backup your whoe jenkins server with os...into vmi, which has all configurations, data and setup inside as configured. 
backlups of the vmi is called SNAPSHOTS.
if vmi is damaged or corrupt, SNAPSHOT can be used to recreate the server easily.

---
---
---


#LVM LOGICAL VOLUME MANAGER
allows creation of groups of disks or partitions that can be assembled into single or multiple filesystesm
used for any mount point excpet /boot
allows resizing volumes
allows to take snapshots... point in time copies of local volume : pauses write for a moment, tkaes snapshot




pv = physical volumes
vg = volume groups
lv = logical volumes



bin = importent executables and core os commands. symbolically linked to usr/bin
boot = files for bootloader. includes ramfiles, kernal
dev = device files
etc = critical configuration files + startup scripts. for managing linux server. to change ssh as well.
home = directory for user files. typical user cannot access the home of another user. terminal opens with home directory.
lib = shared libraries programs need. lib32 and lib64 for diff bits. sym link to usr/
lostnfound = files that are there when system crashes
media = mount point for files stored on removable media
mnt = mount devices temporarily here
opt = rarely used. optional software packages place
proc = psudo file system. createed at startup and dedstroyed at end. info about every running proccess on machine. ton of info. cat /proc/cpu-info can show cpu information

root = home directory for admin user. cannot be accessed without sudo, and root user permissions
run = info since boot times and daemons running
sbin = system essentioals like bin . symbolically linked to usr/sbin
srv = files serverd by server. rarely used.
sys = info about device, drivers, kernal. like proc, but better structure
temp = temporary files that arent needed after next reboot.


usr = most porgrams or utils system is running. place most of the programs reside. all files are stored in here only. usr/bin & usr/sbin are the main locations

var = system specific variable files... temp spool files. web servers place files here


man hier has details about these



everything in linux are files
example..
if you want to see the partitions present in your pc
shortcut coimmand
ls -l /dev/nvme*
this will show an output which tells the partitions for each direv




# SSH
Creating Shell Remotely in a Secure manner

ssh packet
packet length + padding lenght + payload + padding data + mac
4 bytes       + 1 byte         +

padding data is random data mixed in with the payload 
payload is the data
mac = message authentication code



# KVM
Kernal Virtual Machine
This is a shortcut provided in LINUX to make it virtual machine feel like inbetween type 1 and type 2 hypervisor.
KVM allows the QEMU or Virtual box to directly access resources without complicating with additional layer of host OS resource allocation.

