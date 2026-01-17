created a vm with the following settings
- vm name set as my username
- selected the debian iso donwloaded
- unchecked unattended installation 
- next
- 8gb ram, 1 cpu and 30gb storage
- once created, turned on the vm

installing debian
- chose the language and region to be english and united states
- was asked to set hostname, set the hostname as : ssujaude42
- for domain name, left it blank as it is not needed
- setting the password : Suhail@42
- reconfirm the password : Suhail@42
- setting the username : ssujaude
- setting the username again : ssujaude
- setting the user account password : 42@Suhail
- reconfirm the user account password : 42@Suhail

partitioning
- as im doing bonus... i need to to manual partitioning method
- partitioning method : choose manual

- info
    SCSI = Small Computer System Interface
    This is something smarter and efficient than IDE
    It came in the same era as the IDE, but it was more expensive.
    Linux kept using the term SCSI because it is a much more efficient way of data transfer
    even when IDE or other methods came up... they continued to use SCSI and added converter or translator codes or adapters to use SCSI even now. 
    when they say SCSI3 (0,0,0), the 3 indicates the Host adapter id, which is usually 0 and 1 for Motherboard, 2 for cd/dvd or the mounted drive from where the ISO is running.. and 3 for the actual hard drive.
    the (0,0,0) indicates (controller num, bus num, logical unit num)

- select the SCSI3 (0,0,0) which has the entire volume to be partitioned.
- Then as we need to build the partition to install the operating system on... we create the partition...
    - sda1 shown in the subject image needs 500mb
    - therefore now, we select 'Create a new partition'
    - enter the size as 500mb
    - choose the partition type to be 'Primary'

    - info
        Primary Partition is something pretty directly linked to the computer, and boot from it immedietely
        Logical Partition is something like sub slices within a container.
        So in the olden hardware, they used MBR, Master Boot Record, and this kept track of all the partitions and volumes. 
        So, this MBR could only store 4 partitions. this was a set limit. So to overcome it, they used 1 of  the 4 partition slot to be extendable and logical partitions... This way, you can have n number of logical volumes in this one slot, but primarily only 3 main slots were there. If you dont need any logical partitions, then you can still have 4 primary slots. Jumpers were used to tell the address of the drive manually for the computer. 
        Bootloader used to be not able to find the OS in logical volumes. Therefore, it used to be put in Primary Partitions.
        Modern systems use something acalled GPT - GUID Partition TAble, and this has no limit. Every partition is now primary.

    - Location for partition, choose : 'Beginning'
        Beginning indicates the fastest access, and End is mainly leftover space, for things you dont use often
        This was mainly for HDDs

    - Set Mount Point : /boot : as this partition was created for the operating system
        Mount Points are basically the C: D: A: in windows. THis is the Linux Equivalent.
        Its the point when the storage is accessible from.
    
- now the boot partition is ready


creating the logical partitions
- we select the remainiing space
- create a new partition
- set size as 'max'
- choose 'Logical' as these are not the boot drives.
- Set Mount Point, and choose 'Do not mount it'. 

encrypting the volumes
- Configure Encrypted Volumes.
- 'Yes' for partition creation and formatting. 
- 'Create Encrypted Volumes'
- Select the large logical partition we created.
- 'Done setting up the partition'
- Choose 'Yes' for erasing data on SCSI3.
- Cancel the overwriting process as there is no data on it, and its empty. So this is unnecessary.
- Enter a passphrase for the encryption. : 4@Suhail@2
- Once done, it will show the encrypted volume listed.


configuring the LVM - Logical Volume Manager
- 'Configure the Logical VOlume MAnager'
- Accept the confirmation message to format partitions
- 'Create volume group'
- Enter volume group name : LVMGroup
- Select the large encrypted partition we made
- Once created... we need to create Logical Volumes for this group

- info
    logical volume groups could easily create one large volume out of multiple physical storage drives. 


configuring Logical Volumes
- 'Create Logical Volume'
- Choose LVMGroup
- enter the volume name : root
- enter size : 10GB
- similarly create logical volumes for swap, home, var, srv, tmp, var-log

once done, all logical volumes are created
now we need to select the file system and mount point for each

- choose the '#1 5.0GB' which is under home
- select 'Use as: do not use' 
- choose Ext4 journaling file system
- set mount point to \home
- repeat for each one similarly and assign the right mount points.
- special cases are for swap, where the file system to select is 'swap area'
- and for var-log, the mount point is to be entered manually... '/var/log'
- once all done 'Finish partitioning and write changes to disk'
- accept the listed changes

- info
    ext2 : original version. simple and fast. no journal. so if power down, loss of data is imminent.
    ext3 : has the journal with a tiny logbook. if power down, journal helps recover.
    ext4 : modern version. journal is present but its faster and handles huge hdd. very reliable
    exts are the daily driver versions

    xfs & jfs are for massive amounts of data. for large servers
    btrfs (butter fs) takes snapshots of the system so that if some settings messup, you can restore back to 10 mins before

    FAT16 And FAT32 are windwos friendly. but FAT32 cant hold single file with large storage size

    swap area... for virtual memory area

    physical volume for encrytpion = this is a digital vault. whatever data is stored inside this, its always encoded before hand and then stored. every data is scrambled. it scrambles and descrambles automatically.





installation will begine.
- once it asks for 'scan extra installation media', it means to install additional packages. 
- we skip this by 'NO'
- We choose country for Debian mirror
- then choose the archive mirror source... deb.debian.org
- for the proxy, leave it blank
- for survey, NO
- when software selection is shown, deselect everything and continue
- when prompted for GRUB boot loader, choose 'YES'
- choose the /dev/sda

once all is set  = Reboot

for login, it will ask for the passphrase used for encryption : 4@Suhail@2
use login username and password after that

lsblk to view the structure of volume

--- debian virtual machine created successfully
noticed issues with inconsitency in creation of storage partitions.
maybe becuase i mentioned gb instead of allocating with mbs
need to recheck and update later on


----




installing sudo
- switch to root user by using ```su``` command
- enter the password used to create for the hostname
- ```sudo reboot```
- once rebooted, be the root user again
- ```sudo -V``` for checking installation of Sudo

to create a new user...
```sudo adduser <newusername>```
do it as a root user



Groups
    Groups are essential to manage access and permissions and configurations for multiple users at once.

    creating a group
        ```sudo addgroup <groupname>```
        do it as a root user
        if it exists, it will give an alert.
        else, it might give a notification or complete quietly.

        to verify
            ```getent group <groupname>```
            and this will tell the group id is has.
            getent => get entries
        
    adding users to groups
        ```sudo adduser <username> <groupname>```
        for this project, add username to both the new group and sudo as well.
    
        to verify groups and users
            ```getent group <groupname1> <groupname2>```
            and this will show all the users under the group as well.
    

Installing SSH (SECURE SHELL)
    in root user
    sudo apt update
    sudo apt install openssh-secure
    confirm with Y to install
    verify by : sudo service ssh status
    this will show active (running)

    edit ssh configuration
    vim /etc/ssh/sshd_config
    find #Port 22 and change to Port 4242
    Find: #PermitRootLogin prohibit-password & Change to: PermitRootLogin no
    save

    vim /etc/ssh/ssh_config
    find #Port 22 and change to Port 4242
    save

    restart and check status
    sudo service ssh restart
    sudo service ssh status
    in the logs it will show that 4242 port is active.

    once the service is active, need to configure the VM to be able to send data out to the main host pc
    Goto VirtualBox, select the VM created, Configuration menu, and select the Network tab, and click on Port Forwarding.
    Click the add rule icon, and change the host port and guest port to 4242.
        Sometimes this may not work, and you may need to put 4241 & 4242.  dont know why... we need to find out.
    Click ok.
    Now we test if all is set and working...
    ssh <username>@localhost -p 4241
    this will connect the terminal to the vm machine like a server.
    enter the username password


Installing UFW & Firewall
    install firewall 
    ```sudo apt install ufw```
    
    enable the firewall
    ```sudo ufw enable``` 

    allow a port to firewall for 4242 port
    sudo ufw allow 4242

    check status
    sudo ufw status

Configuring sudo policies
    Create directory /var/log/sudo
    Create the file for the sudo configuration /etc/sudoers.d/sudo_config
    Config file
    ```
    Defaults  passwd_tries=3
    Defaults  badpass_message="Incorrect password given!"
    Defaults  logfile="/var/log/sudo/sudo_config"
    Defaults  log_input, log_output
    Defaults  iolog_dir="/var/log/sudo"
    Defaults  requiretty
    Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
    ```

    passwd_tries=3: Total tries for entering the sudo password.
    badpass_message="message": The message that will show when the password failed.
    logfile="/var/log/sudo/sudo_config": Path where will the sudo logs will be stored.
    log_input, log_output: What will be logged.
    iolog_dir="/var/log/sudo": What will be logged.
    requiretty: TTY become required
    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin": Folders that will be excluded of sudo



Setting up Password policy
    vim /etc/login.defs
    Change: PASS_MAX_DAYS 99999 → PASS_MAX_DAYS 30
    Change: PASS_MIN_DAYS 0 → PASS_MIN_DAYS 2
        PASS_MAX_DAYS: Maximum number of days before password expires.
        PASS_MIN_DAYS: Minimum number of days before password can be changed.
        PASS_WARN_AGE: Number of days before password expiration to show warning.
    
     To enforce password quality rules, install the following package:
     sudo apt install libpam-pwquality

     Once installed, we need to configure the Pluggable Authentication Modules config file.
     vim /etc/pam.d/common-password

     find the line with retry=3, and add the following
     minlen=10 ucredit=-1 dcredit=-1 lcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root
     minlen = minimum password length
     ucredit = uppercase character requirement. -1 indicates atleast 1
    lcredit = lowercase character requirement. -1 indicates atleast 1
    dcredit = digit character requirement. -1 indicates atleast 1
    maxrepeat = indicates how many times a character can repeat together
    reject_username = the username itself cant be the password
    difok = means number of characters that has to be different, compared to the old password
    enforce_for_root = even the root cannot bypass these laws.
