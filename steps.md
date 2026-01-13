Install VirtualBox
Download Debian ISO

Creating Virtual Machine
1. Click New
2. Add a name for the VM, Choose the ISO, and click next.
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

