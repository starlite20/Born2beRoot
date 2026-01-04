🚀 Born2beRoot Progress Tracker

Progress Overview

🛠️ Virtual Machine Setup
✅ Download Debian ISO
✅ Install VirtualBox
Create new VM (8GB RAM, 30GB disk)
Configure VM settings and attach ISO
Boot VM and verify ISO loading

🌀 Debian Installation
Start Debian installation process
Configure language, country and keyboard
Configure network and hostname ([login]42)
Set domain name (leave empty)
Set root password (strong password)
Create user account ([login]42)
Configure LVM with encryption (manual partitioning)
Configure package manager (no extra software)
Install GRUB bootloader and finish

⚙️ System Configuration
First login and system verification
Update system packages (apt update & upgrade)
Install and configure sudo with strict rules
Add user to sudo and user42 groups
Install OpenSSH, configure port 4242, disable root login
Install UFW, enable port 4242, configure firewall
Configure strong password policy (pwquality)
Verify hostname and implement hostname change script

🧾 Monitoring Script
Create monitoring.sh with system information display
Set proper script permissions and location
Test script functionality and output format
Configure crontab for 10-minute intervals

✅ Evaluation Preparation
Generate signature.txt file for submission
Prepare and test evaluation commands
Complete system functionality test
Submit project on intra

😊 Bonus: WordPress Setup
Set up additional partition structure
Install and configure MariaDB database
Install Lighttpd web server and PHP
Download and configure WordPress
Test WordPress functionality and access
