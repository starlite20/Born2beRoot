# My VirtualBox & Debian Setup Log

## The Initial Headache: VMX Conflict
I started by trying to fire up VirtualBox for my Debian install, but I immediately hit a wall with the error: `VERR_VMX_IN_VMX_ROOT_MODE`. 

**What I learned:** It turns out my CPU’s virtualization "engine" (VMX) is like a driver's seat that only fits one person. Since I'm on Linux, the built-in KVM hypervisor had already hopped in and locked the door. To let VirtualBox take the wheel, I had to manually kick KVM out for the session.

**The Fix:**
I checked if KVM was running with `lsmod | grep kvm`, then used `sudo modprobe -r` to unload the modules (specifically `kvm_intel` for my chip). It’s not permanent—a reboot brings KVM back—but it cleared the path for VirtualBox to finally start.

---

## Setting up the Virtual Machine
I kept the specs modest since the project doesn't need much power.
* **ISO:** Grabbed the Debian 12 "netinst" (64-bit).
* **Config:** Named the VM, pointed to the ISO, and made sure to hit **"Skip Unattended Installation"**—I want to handle the setup myself.
* **Resources:** Gave it **1 CPU** and kept the base memory reasonable.
* **Disk:** Created a fresh **8GB** virtual disk as required.

---

## Installation & Partitioning (The Manual Way)
I skipped the Graphical Install and went with the standard **"Install"** mode. 
* **Identity:** Set the hostname as `my_login42` and skipped the domain.
* **Users:** Created a root password and my own user account.

### Dealing with the Disk (The Hard Part)
This is where it got technical. I had to set up encryption and LVM manually, which is way more secure than the "automatic" options.

1. **Splitting the Drive:** I made a small **500MB primary partition** for `/boot`. This has to stay unencrypted so the computer knows how to start. Everything else went into a big **logical partition**.
2. **Encryption:** I used the "Configure encrypted volumes" tool on that big logical partition. I set a strong passphrase that I’ll need every time I turn the machine on.
3. **LVM (Logical Volume Manager):** Inside that encrypted space, I created a Volume Group called `LVMGroup`. This acts like a "virtual disk" that I can carve into smaller pieces.

### My Volume Map:
I divided the space specifically to keep the system organized and prevent one folder (like logs) from filling up the whole drive:
* **root (/)**: 2.8G
* **home**: 2G
* **swap**: 1G
* **tmp**: 2G
* **srv**: 1.5G
* **var**: 2G
* **var-log**: 2G (Manually typed the mount point for this one)

**Final Check:** I went through each logical volume, set the filesystem to **Ext4**, and assigned the correct mount points. Once it looked right, I committed the changes to the disk and confirmed.

---

**Next Step:** I'm finishing the base install now. Next up is setting up **Sudo** permissions and locking down the **UFW** firewall.






Install VirtualBox
Download Debian ISO
Visit the Debian download page and download the latest stable version of a 64-bit small installation image (e.g., debian-12.8.0-amd64-netinst.iso).

Creating Virtual Machine
1. Click New
2. Add a name for the VM, Choose the ISO, and make sure to check 'Skip Unattended Installation' click next.
3. Update username and password
4. Set Base Memory as needed, and 1 CPU is enough.
5. Choose Create Virtual Hard Disk Now, and set 8GB as that is given in subject.

For my laptop, it didnt run immedietely. It had an issue, so to resolve, I had to do the following.
lsmod | grep kvm
sudo modprobe -r kvm_intel
sudo modprobe -r kvm
This is done because modern cpus have virtual machine extensions (vmx). THis allows the VM to run fast like a real computer. Intel and AMD  have added one more strict rule to allow only one hypervisor that can control cpu virtualization seat. 
KVM is the linux builtin hypervisor. As its part of the OS, it usually sits in the seat when it boots up. Therefore we remove using the commands.

If you need KVM back, you don't even have to reboot. You can simply run sudo modprobe kvm_intel (or amd) to reload it, or just restart your computer, and the system will load it back automatically.





    Select Debian ISO image as startup disk.
    When Debian starts, choose Install, not graphcal install.
    Choose language, geographical & keyboard layout settings.
    Hostname must be your_login42 (ex. mcombeau42).
    Domain name leave empty.
    Choose strong root password & confirm.
    Create user. your_login works for username & name.
    Choose password for new user.


1. Choose ```Manual``` partitionning.
2. Choose sda Harddisk - ```SCSI (0,0,0) (sda)``` ...
3. ```Yes``` create partition table.

We will crete 2 partitions, the first will be for an unencrypted /boot partition, the other for the encrypted logical volumes :
* ```pri/log xxGB FREE SPACE``` >> ```Create a new partition``` >> ```500 MB``` >> ```Primary``` >> ```Beginning``` >> ```Mount point``` >> ```/boot``` >> ```Done```.
* ```pri/log xxGB FREE SPACE``` >> ```Create a new partition``` >> ```max``` >> ```Logical``` >> ```Mount point``` >> ```Do not mount it``` >> ```Done```.

### Encrypting disks
1. ```Configure encrypted volumes``` >> ```Yes```.
2. ```Create encrypted volumes```
3. Choose ```sda5``` ONLY to encrypt. We DO NOT want to encrypt the ```sda /boot``` partition.
4. ```Done``` >> ```Finish``` >> ```Yes```.
5. ... wait for formatting to finish...
6. Choose a strong password for disk encryption. DO NOT forget it!

