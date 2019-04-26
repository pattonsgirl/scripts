# UEFI / Secure Boot & VirtualBox
## Linux Host, Windows 10 Client
* At time of writing, `apt install virtualbox` installed version 5.2.18

### Fixing crash reports
* Note: This fixes System Crash reports after installing virtualbox \
I do not disable future crash reports by fixing this 
  * `sudo ls /var/crash/` 
  * `sudo rm /var/crash/_usr_lib_virtualbox_VirtualBox.0.crash`

### Installing virtualbox
* Install virtualbox from repo \
`sudo apt install virtualbox`
* Note: For UEFI, you will be asked to set a password for MOK \
`sudo reboot`
* On reboot, a BIOS screen will come up for MOK management \
enter password you just made in the virtualbox installer
  * IF YOU MISS THIS, run `sudo mokutil --password`, then `reboot`
* In UEFI / Secure Boot mode, kernel drivers need to be signed
  * For virtualbox, this means we need to sign `vboxdrv`
#### Signing Kernel Module for vboxdrv
* Create key:
  * `openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=Descriptive common name/"`
* Sign the module:
  * `sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxdrv)`
* Confirm module is signed:
  * `tail $(modinfo -n vboxdrv) | grep "Module signature appended"`
* Register keys to Secure Boot:
  * `sudo mokutil --import MOK.der`
* Reboot, make sure you select "Enroll MOK"
* Confirm key is enrolled:
  * `mokutil --test-key MOK.der`

#### To Access USB Drives within Client VM
* Check your virtualbox version
  * `virtualbox --version`
* Download extension pack for your version
  * `wget https://download.virtualbox.org/virtualbox/5.2.18/Oracle_VM_VirtualBox_Extension_Pack-5.2.18.vbox-extpack`
* In the virtualbox GUI, go to File -> Preferences -> Extenstions
  * Select add new package, navigate to your .vbox-extpack
  * Select OK
* Add your username to the `vboxusers` group \
`sudo usermod -aG vboxusers <username>`
  * `vboxusers` allows access to USBs within the VM
* Recommend at minimum logging out and back in, or reboot Host
* Select your Host VM, then select Settings
  * Go to USB, then select up to 3.0
  * From this menu OR power on VM, go to Add New USB Filter
  * Select USB you would like to mount in Client

### Installing Windows 10 Client on Linux virtualbox Host
* Follow Step 3 and after in this guide
  * https://itsfoss.com/install-windows-10-virtualbox-linux/

### Windows 10 Enterprise for Virtual Machines
#### Create account instructions
* From the sign in screen, press and hold the SHIFT key on the keyboard. \
With this key still pressed, click or tap the Power button and, in the menu \
that opens, click Restart.
* Windows 10 restarts and asks you to select an option. \
Choose Troubleshoot.
* On the Troubleshoot screen, go to Advanced options.
* On the Advanced options screen, choose Startup Settings. \
Depending on your Windows 10 computer, you may not see this option at first. \
If you do not, click or tap the link that says "See more recovery options."
* Click or tap the Startup Settings option
* Windows 10 says that you can restart your device to change advanced boot options, \
including enabling Safe Mode. \
Press Restart.
* After Windows 10 restarts one more time, you can choose which boot options \
you want to be enabled. \
To get into Safe Mode, you have three different options:
  * Standard Safe Mode - press the 4 or the F4 key on your keyboard to start it
  * Safe Mode with Networking - press 5 or F5
  * Safe Mode with Command Prompt - press either 6 or F6 (I CHOOSE THIS ONE)
* Windows boots up into the command prompt. THIS CAN TAKE AWHILE
* The next step is to create an account and add this one to the local admin group. \
Enter the following commands:
  * `net user username /add`
  * `net localgroup administrators username /add`
* Now reboot VM and log in with username - no password

### Resources
* Crash Reports
  * https://www.binarytides.com/ubuntu-fix-system-program-problem-error/
* Add kernel module for virtualbox in Ubuntu
  * https://flavioprimo.xyz/blog/how-to-install-virtualbox-on-ubuntu-having-uefi-secure-boot-enabled/
  * https://askubuntu.com/questions/760671/could-not-load-vboxdrv-after-upgrade-to-ubuntu-16-04-and-i-want-to-keep-secur
* Install Win 10 VM in Linux Host
  * https://itsfoss.com/install-windows-10-virtualbox-linux/
* How to create account in Win 10 Ent for Virtual Machines
  * https://techcommunity.microsoft.com/t5/Windows-Server-for-IT-Pro/Windows-10-build-1809-Enterprise-for-Virtual-Machines-Local/td-p/293250