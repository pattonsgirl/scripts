# UEFI & VirtualBox
## Linux Host, Windows 10 Client
### Fixing crash reports
* Note: This fixes System Crash reports after installing virtualbox \
I do not disable future crash reports by fixing this 
  * `sudo ls /var/crash/` 
  * `sudo rm /var/crash/_usr_lib_virtualbox_VirtualBox.0.crash`

### Installing virtualbox
  * `sudo apt install virtualbox`
* Note: For UEFI, you will be asked to set a password for MOK
  * `sudo reboot`
* On reboot, a BIOS screen will come up for MOK management \
enter password you just made in the virtualbox installer


### Windows 10 Enterprise for Virtual Machines
#### Create account instructions
* From the sign in screen, press and hold the SHIFT key on the keyboard. With this key still pressed, click or tap the Power button and, in the menu that opens, click Restart.
* Windows 10 restarts and asks you to select an option. Choose Troubleshoot.
* On the Troubleshoot screen, go to Advanced options.
* On the Advanced options screen, choose Startup Settings. Depending on your Windows 10 computer, you may not see this option at first. If you do not, click or tap the link that says "See more recovery options."
* Finally, click or tap the Startup Settings option
* Windows 10 says that you can restart your device to change advanced boot options, including enabling Safe Mode. Press Restart.
* After Windows 10 restarts one more time, you can choose which boot options you want to be enabled. To get into Safe Mode, you have three different options:
  * Standard Safe Mode - press the 4 or the F4 key on your keyboard to start it
  * Safe Mode with Networking - press 5 or F5
  * Safe Mode with Command Prompt - press either 6 or F6 (I CHOOSE THIS ONE)
* Windows boots up into the command prompt. THIS CAN TAKE AWHILE
* The next step is to create an account and add this one to the local admin group. Therefore, you need to enter the following commands:
  * `net user username /add`
  * `net localgroup administrators username /add`
* Now reboot VM and log in with username - no password

### Resources
* Crash Reports
  * https://www.binarytides.com/ubuntu-fix-system-program-problem-error/

* How to create account in Win 10 Ent for Virtual Machines
  * https://techcommunity.microsoft.com/t5/Windows-Server-for-IT-Pro/Windows-10-build-1809-Enterprise-for-Virtual-Machines-Local/td-p/293250