# HP-Z620-macOS-Catalina
## Complete EFI for HP Z620 for macOS Catalina 10.15.5 using Clover 5119.

## UPDATES: Check the repo for EFI directory updates for the latest version.

### HP Z620 Catalina Install Guide

Hardware Overview:

- Model Name:	Mac Pro (HP)
- Model Identifier:	MacPro6,1 (Z620)
- Processor Name:	8-Core Intel Xeon E5
- Processor Speed:	2.69 GHz
- Number of Processors:	1
- Total Number of Cores:	8
- L2 Cache (per Core):	256 KB
- L3 Cache:	20 MB
- Hyper-Threading Technology:	Enabled
- Memory:	128 GB
  
  
GPU: (NOTE: If you DO  have an AMD Radeon 5700 XT ADD the boot flag -agdpmod=pikera, otherwise you'll get a black screen)
  
- Chipset Model:	Radeon RX 5700 XT
- Bus:	PCIe  
- Slot:	Ethernet  
- PCIe Lane Width:	x16  
- VRAM (Total):	8 GB  
- Vendor:	AMD (0x1002)
- Device ID:	0x731f
- Revision ID:	0x00c1
- ROM Revision:	113-230LNAVIXT612_8G
- Metal:	Supported, feature set macOS GPUFamily2 v1

#### Things you’ll need:

1. A USB drive that is 16GB or greater

2. A HP Z620 Workstation with a graphics card that is natvely supported by macOS Catalina. A definitive list can be found here https://support.apple.com/en-gb/HT208544

3. Access to an Apple Macintosh to download macOS Catalina from Apple via the AppStore https://apps.apple.com/gb/app/macos-catalina/id1466841314?mt=12

4. Clover Bootloader https://github.com/CloverHackyColor/CloverBootloader/releases

5. Clover Configurator https://mackie100projects.altervista.org/download-clover-configurator/

5. HP Z620 EFI.zip - EFI directory files from this GitHub repo above

#### USB Installer Prep: 

1. Take your USB drive and plug it in to your working Macintosh.

2. In the top right-hand corner, click the 'Find' icon, and then type 'Disk Utility' then click on the 'Disk Utility.app' application to open it. 

3. Locate your installer USB drive you just plugged in to the Macintosh and format it as:
- Mac OS Extended (Journaled)
- GUID Partition Map
- Give it the name; ‘Install macOS Catalina’, then hit erase

4. Download Catalina on your Mac from the Apple AppStore. It will download to your Applications folder as 'Install macOS Catalina.app'

5. Open Terminal and type the following command:

***sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/Install \macOS \Catalina —nointeraction***

You will be prompted for your password to the Macintosh, enter this then hit 'Enter'. You will prompted for a ‘y’ confirmation prior to copying the installer files to your USB drive. Wait until the process completes, it will take several minutes. Go and have a cup of tea.
 
6. When the above process has completed, download the Clover bootloader package and install it to your Catalina Installation USB device **NOT THE MACINTOSH YOU'RE CURRENTLY WORKING ON!!** using the following options; 'Clover for UEFI booting only’ & 'Install Clover in ESP’, leave any default options checked.

7. Extract the 'HP Z620 EFI.zip' folder downloaded from GitHub. Copy the contents to the root of your boot drive's EFI mounted partition, the same location to where you just installed the Clover bootloader. Overwrite or delete the existing EFI folder that’s already there prior to pasting the contents of 'HP Z620 EFI.zip'

#### BIOS Prep:

Set your BIOS (v3.90) as follows: (F10 to Enter BIOS)

Storage > Storage Options
- Removal Media Boot = Enable
- SATA Emulation = AHCI

Storage  > Boot Order
- UEFI = USB Hard drive at the top
- Legacy = USB Hard Drive at the top

Security > Device Security
- Embedded Device Security = Device Hidden
- Everything Else = Enable

Security > USB Security
- Restrict USB Devices = Allow all USB Devices
- Everything Else = Enabled

Security > Slot Security
- Enable all

Security > Network Boot
- Network Boot = Disabled

Security > System Security
- Data Execution Prevention = Enable
- Virtualisation (VTx) = Enable
- Virtualisation (VT-d) = Disable

Security > Secure Boot Configuration
- Legacy Support = Enabled
- Secure Boot = Disabled
- Fast Boot = Disabled

Security > Device Security
- Serial port = Hidden
- Internal USB = Hidden
- SAS Controller = Hidden
- Legacy Diskette = Hidden
- Embedded Security = Hidden

Power > OS Power Management
- Runtime Power Man = Enable
- Turbo Mode = Enable
- Idle Power savings = Extended
- PCIe Performance Mode = Disabled
- Unique Sleep State Blink Rates = Disabled

Power > Hardware Power Management
- S5 Maximum Power Savings = Disabled

Advanced > Processor
- Hyper-Threading = Enable
- Active Cores = All Cores
- Limited CPUID = Disable

Advanced > Device Options
- Num Lock = On
- S5 Wake on LAN = Enable
- Multi-Processor = Enable
- Internal Speaker = Enable
- Mass Storage Option ROMS = Legacy
- Video Option ROMS = Legacy
- Active Processor Cores = All
- Hyper Threading = Enabled
- CECP Mode = Disabled

#### Install macOS Catalina:

1. Unplug the installer USB drive from the Macintosh and plug it in to the USB 2.0 port on the front of the Z620

2. Power the Workstation on and select 'Install macOS Catalina' from the Clover boot menu. 

3. Using Disk Utility when you reach the installer on the Z620, ensure you’ve formatted the destination drive (the disk you are installing to) in the Z620 using the follwing settings:

- Mac OS Extended (Journaled)
- GUID Partition Map
- name this macOS Catalina (or whatever your preference is)

Now, continue to install OSX as you normally would, navigating through the  Apple installation process. Your machine will restart a number of times during the install process, this is normal. 

#### Post Catalina Install:

###### Installing the Clover bootloader to your macOS Catalina boot drive

So far, the modified bootloader is only present on the USB. This means without the USB drive present in your Z620, your newly installed Catalina will not boot on the workstation without it. We must now install clover to create EFI partition on the macOS Catalina boot drive.

1. Once you’ve booted in to 'macOS Catalina' on your workstation and you’ve reached the desktop, copy the Clover bootloader installer package over to your HP Z620 'macOS Catalina' desktop and install it to the boot drive, same as you did in step 6 of the USB Prep phase above, **this time changing the destination of the install to 'macOS Catalina'** (or whatever you named your macOS boot drive). Once completed move on to step 2.

2. Extract the downloaded 'HP Z620 EFI.zip' file from GitHub. 

3. Change the Serial Number for your new Hackintosh from the one that is included in the 'config.plist' file in the downloaded 'Hp Z620 EFI.zip'. Otherwise, multiple accounts using the same serial number will lock your AppleID. To change the Serial Number;

- Locate and open the 'config.plist' file in the recently extracted EFI folder.
- On the left-hand menu, click on 'SMBIOS', then on the bottom right-hand corner of the main pane, ~~click on 'Generate New' next to the 'serial number'~~ click the 'up/down' arrow and generate a new System Definition field. You can check if the serial generated is free to use by clicking on 'Check Coverage' on the right of 'Clover Configurator'. Apples website will tell you if it's already in use. Thanks Apple ;-)
- Press cmd+S to save the file change.

###### Now we will copy the modified EFI directory to the macOS Catalina boot drives' EFI partition.

1. Download the 'Clover Configurator' application. Install it to your Applications directory and open it. 

2. Mount the EFI partition if it's not already mounted) for your 'macOS Catalina' boot drive otherwise skip to step 3.

3. Copy your modified EFI folder (with the serial changed) and paste it over the one that's already there, replacing the EFI folder on Z620 boot drive. 

4. Safety remove your Installer USB and restart your workstation.

Enjoy your Hackintosh Z620.
