# Device Support Reference Guide

# Overview

This document provides general guidance to Device Administrators within any organization.

# Mobile Devices

## All Mobile Device Types

### Restoring a Mobile Device with Face ID

1. Plug device into computer and open Apple Configurator 2
2. Put device into Recovery Mode
3. Quickly press and release **volume-up**
4. Quickly press and release **volume-down**
5. Hold down **Power/Side** Button. Keep holding until device restarts, shows Apple logo, then shows recovery screen
6. Right-click on device in Apple Configurator 2 and select **Restore**

### Exiting Recovery Mode

1. Unplug device from computer
2. Hold down power button until device restarts

## iPhones

### Using an Apple ID Temporarily to Install Apps

1. Open **App Store**
2. Click on **account** in top-right
3. Select **Sign Out**
4. Enter Apple ID credentials and **sign in**
5. Select **Done**
6. Search for and install the necessary app
7. Verify app has been installed on device

When complete, sign out of the Apple ID and sign back into the Managed Apple ID

1. Open **App Store**
2. Click on **account** in top-right
3. Select **Sign Out**
4. Sign in with the managed Apple ID
5. Select **Done**

# Computers

## Allow Boot Media on Macs with T2 Chip

If Mac has TouchID, it is T2 chip enabled. Follow these steps before using a bootable drive to restore. 

1. Boot into Internet Recovery holding **Command-Option-R** while powering computer on 
2. Connect to a WiFi network
3. Select **Utilities** > **Startup Security Utility**
4. Set **allowed boot media**

## Create a Bootable Installer for macOS

Instructions taken from: https://support.apple.com/en-us/HT201372

1. On a host computer, download the macOS version from the App Store
2. Verify installer is in Applications folder
3. Verify title is `Install macOS <version_name>`
4. Connect USB flash drive to host computer

Check USB flash drive format and format if necessary

1. Open **Disk Utility**
2. Select the external hard drive

If needed, format:

1. Select **Erase**
2. Format: Mac OS Extended (Journaled)
3. Name: macOS Installer
4. Select **Erase**

Use createinstallmedia Terminal command

1. Open Terminal and enter following command

```
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/macOS\ Installer
```

2. Enter host computer password
3. Follow prompts in Terminal window
4. Select 'Y'

## Erasing a Computer

Instructions taken from [here](https://support.apple.com/guide/mac-help/erase-and-reinstall-macos-mh27903/mac).

### Erasing a Computer without TouchID

1. Plug computer into network via Ethernet if possible
2. Power computer off
3. Power computer on while holding **Command-R**. Release Command-R when Recovery Screen loads. 
4. If Recovery Screen does not load, follow steps to Reset PRAM and try again
5. If you are prompted to Select a user you know the password for, select Recovery Assistant > Erase Mac... and Erase Mac
6. When successful, a flashing folder icon will appear

## Install Docker

1. On computer, go to https://docs.docker.com/docker-for-mac/install/
2. Select **Mac with Intel Chip**
3. Select **Allow**
4. After download is complete, double-click on Docker.dmg installer
5. Drag Docker to Applications folder
6. Open Docker application
7. Select **Open**
8. Select **OK** to install helper
9. Enter computer password
10. Accept Terms and Conditions

## Install HomeBrew

1. Open Terminal and run following:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. If you get a 'failed to link all completions error', enter the following:

```
sudo chown -R $(whoami): /usr/local/share/zsh
```

3. Verify install:

```
brew doctor
```

## Install Python3

1. Ensure HomeBrew is installed by opening Terminal and running:

```
brew doctor
```

2. Open Terminal and run following:

```
brew install python3
```

## Manually Enrolling a Computer into PreStage After Setup

1. Connect computer to an approved WIFi network.
2. Open Terminal and enter following:

```
sudo profiles renew -type enrollment
```

3. Computer should be prompted to enroll into organization. Select **Allow**.
4. System Preferences should open and prompt to install enrollment profile. 

## Performing a PRAM Reset

Reseting PRAM (also known as NVRAM or NonVolatile Random Access Memory) clears a small amount of memory that stores certain settings for fast access, such as:

- Sound volume
- Display resolution
- Startup-disk selection
- Time zone
- Recent kernel panic information

Perform PRAM reset by doing the following:

1. Power computer off
2. Hold down **option-command-p-r**  (this can be done with your right hand by holding option & command with your thumb, R with your index finger, and P with your pinky finger)
3. While holding those buttons down, press and release the power button with your other hand
4. The computer will turn on and restart itself (if you look closely at the screen, you'll see the backlight turn on and off)
5. Let the computer restart itself a few times, then let go of all keys

## Reinstalling macOS using Bootable Installer

### Intel Processor Mac

1. Plug bootable installer into Mac
2. Power computer off
3. Power computer on holding Option
4. Release Option key when you see a dark screen showing bootable volumes
5. Select the **Install macOS ...** volume
6. Select language if prompted
7. Select **Install macOS ...** from Utilities window
8. Connect to a network
9. Installer will load:
10. Select **Disk Utility** and **Continue**
11. In the Menu Bar, select **View** > **Show All Devices**. 
12. Select the Parent Disc, titled **Apple SSD**
13. Select **Erase**
14. Enter following and Erase: Name - Macintosh HD; Format - APFS; Scheme - GUID Partiion Map
15. When complete, click the red X icon to exit Disk Utility
16. Select **Reinstall macOS Monterey** and **Continue**
17. Select **Continue**
18. Select **Agree**
19. Select the **Macintosh HD** and Continue to begin the reinstall

## Reinstalling macOS using Internet Recovery

1. Plug computer into ethernet if possible
2. Power computer off
3. While holding down **Command-Option-R**, power the computer back on. 
4. If recovery screen does not display, perform a PRAM reset and try again.
5. If prompted, choose a network. 
6. The Internet Recovery partition will load
7. If you receive an error, follow steps to **Reinstall macOS using Original Internet Recovery**.
8. If Internet Recovery loads properly, continue on.
9. Select **Disk Utility** and **Continue**
10. In the Menu Bar, select **View** > **Show All Devices**. 
11. Select the Parent Disc, titled Apple SSD
12. Select **Erase**
13. Enter following and Erase
14. Name: Macintosh HD
15. Format: APFS
16. Scheme: GUID Partition Map
17. When complete, click the red X icon to exit Disk Utility
18. Select **Reinstall macOS Monterey** and **Continue**
19. Select **Continue**
20. Select **Agree**
21. Select the **Macintosh HD** and **Continue** to begin the reinstall

## Reinstall macOS using Original Internet Recovery

1. Plug computer into ethernet if possible
2. Power computer off
3. While holding down **Command-Option-Shift-R**, power the computer back on.
4. If recovery screen does not display, perform a PRAM reset and try again.
5. If prompted, choose a network.
6. The Internet Recovery partition will load. Select **English**
7. Select **Disk Utility** and **Continue**
8. In the Menu Bar, select **View** > **Show All Devices**. 
9. Select the Parent Disc, titled **Apple SSD**
10. Select **Erase**

Enter following and Erase

1. Name: Macintosh HD
2. Format: APFS
3. Scheme: GUID Partition Map
4. When complete, click the red X icon to exit Disk Utility
5. Select **Reinstall macOS** and **Continue**
6. If you get a network error, select the WiFi icon in the top-right and connect to a network
7. Select **Continue**
8. Select **Agree**
9. Select the **Macintosh HD** and **Continue** to begin the reinstall

## Reset Forgotten Computer Password

After entering the incorrect password multiple times, you should be presented with an option to Reset it using your Apple ID
 
1. If you are not prompted, select the **?** icon to the right of the password field
2. If still not prompted, force power off your computer, power it back on, and try again
3. If still not prompted, you will need to have your computer restored. Contact support.

Computer should restart and prompt to enter your Apple ID credentials. Enter your Apple ID.

1. If it says your password is incorrect, contact support.
2. Select your user account to reset password for
3. Enter a new password 
4. Password reset should be complete