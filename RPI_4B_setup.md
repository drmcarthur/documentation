# Setting Up a Raspberry Pi 4B (8GB)

## Resources
* [Raspberry Pi Official Documentation](https://www.raspberrypi.org/documentation/)
    * [GPIO](https://www.raspberrypi.org/documentation/usage/gpio/)
    * [Usage](https://www.raspberrypi.org/documentation/usage/)
    * [Boot modes](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/README.md)
        * [Boot from USB](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/msd.md#:~:text=Note%20that%20although%20the%20option,see%20USB%20device%20boot%20mode.)
    * [Power supply](https://www.raspberrypi.org/documentation/hardware/raspberrypi/power/README.md)
    * [USB](https://www.raspberrypi.org/documentation/hardware/raspberrypi/usb/README.md)
* [Raspberry Pi 4 Getting Started](https://www.youtube.com/watch?v=BpJCAafw2qE)
    * [Crosstalk solutions blog](https://crosstalksolutions.com/getting-started-with-raspberry-pi-4/)

## Install Raspberry Pi OS w/NOOBS (SD card)
1. Download NOOBS (New-Out-Of-the-Box Software) from [here](https://www.raspberrypi.org/downloads/raspberry-pi-os/)
1. Unzip the files and navigate to the NOOBS folder (e.g. `https://www.raspberrypi.org/downloads/noobs/`)
1. Copy all of the contents onto a micro SD card (FAT32 format)
    * Recommended card: [Sandisk Extreme 32 GB (UHS-3)](https://www.amazon.com/gp/product/B06XWMQ81P/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
1. Insert the SD card into the Raspberry Pi 4 and power on
1. Select the first option `Raspberry Pi OS Full`
    * Tip: For more OS options, connect to internet via Ethernet
1. Click `Install` at the top left corner of the screen
    * The install process takes less than 10 minutes (~ 8 min.)
1. After the installation is complete and the Raspberry Pi reboots, follow the prompts to set up the language, time zone, etc.
1. Follow the prompts to install the latest software updates (takes about 15 minutes)

## Install Raspberry Pi OS or Ubuntu 20.04 LTS w/Raspberry Pi Imager
1. Install Raspberry Pi Imager from [raspberrypi.org/downloads](https://www.raspberrypi.org/downloads/)
    * [Raspberry Pi Imager for macOS](https://downloads.raspberrypi.org/imager/imager.dmg)
1. Start Raspberry Pi Imager and Insert SD card
1. Select `Erase` under Operating System (to Format SD card as FAT32)
    * If interested, run [Blackmagic Disk Speed Test](https://apps.apple.com/us/app/blackmagic-disk-speed-test/id425264550?mt=12) to see how fast SD card Read/Write is
1. After formatting, Select the desired operating system in the `Operating System` dropdown
    * NOTE: For some reason, installing Raspberry Pi OS from the imager results in constant crashing... see [alternate method below](#raspberry-pi-os)
1. Select the SD card, then click `Write`
1. Remove SD card and insert it into Raspberry Pi
1. Power on and follow any on-screen instructions
    * NOTE: Make sure Ethernet is plugged in to Raspberry Pi for initial setup
1. See below for instructions depending on OS
 
## Install Raspberry Pi OS using Etcher (supports Adobe Flash in Chromium browser)
1. Download the OS image from [here](https://www.raspberrypi.org/downloads/raspberry-pi-os/)
    * [Raspberry Pi OS (32-bit) with desktop and recommended software](https://downloads.raspberrypi.org/raspios_full_armhf_latest)
1. Erase/Format SD card as FAT32 with Disk Utility (Mac)
1. Use Etcher to write OS image onto SD card
1. Power on and follow on-screen setup instructions
    * Note the audio fixes under [Troubleshooting](#troubleshooting) below

## Install Ubuntu 20.04 LTS using Etcher
* [Reference](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview)
1. Download OS image from [here](https://ubuntu.com/download/raspberry-pi): [Ubuntu 20.04 LTS (64-bit RPi version)](https://ubuntu.com/download/raspberry-pi/thank-you?version=20.04&architecture=arm64+raspi)
1. Erase/Format SD card as FAT32 with Disk Utility (Mac)
1. Use Etcher to write OS image onto SD card
1. Log in with the default username and password: `ubuntu | ubuntu`
1. `sudo apt update`, then `sudo apt full-upgrade` (takes ~ 8 min.)
    * NOTE: If get error about: `Waiting for cache lock: Could not get lock...` it is most likely due to unattended upgrades...e.g. python3 installs. Wait for these upgrades to finish before continuing
1. Install MATE desktop: `sudo apt install ubuntu-mate-desktop` (takes ~ 45 min.)
    * [Release notes for Ubuntu MATE 20.04](https://ubuntu-mate.org/blog/ubuntu-mate-focal-fossa-release-notes/)
1. When prompted, select `lightdm` instead of `gdm3` for Display Manager
1. Wait for install to complete (total desktop install takes about 45 minutes)
1. `sudo reboot`
1. At the login screen, click the Ubuntu logo and select `MATE` desktop environment
1. Log in
1. The display tends to crash if the Raspberry Pi goes to sleep or blanks the display, so disable these settings:
    * Go to `Menu` -> `Preferences` -> `Power Management`
    * Set `Put display to sleep when inactive for: Never`
    * Set `Put computer to sleep when inactive for: Never`
    * Go to `Menu` -> `Preferences` -> `Screensaver`
        * Select a screensaver to use and set the desired idle time
1. Install the Chromium browser: `sudo apt install chromium-browser`
1. Install Kate code editor: `sudo apt install kate`
1. Install hardinfo to monitor hardware: `sudo apt install hardinfo`
1. To pin applications in upper panel, drag icon from applications menu
1. Update Terminal settings: infinite scrollback; background transparency

## Overclocking
* [Reference](https://magpi.raspberrypi.org/articles/how-to-overclock-raspberry-pi-4)
* [Very thorough analysis of different speeds and temperatures](https://qengineering.eu/overclocking-the-raspberry-pi-4.html)
1. Check the current CPU speed
    * One time: `vcgencmd measure_clock arm`
    * Monitor: `watch -n 1 vcgencmd measure_clock arm`
1. Determine the desired over-voltage and frequency settings:
    * For example, 1.75 GHz can be achieved with:
        * `over_voltage=2`
        * `arm_freq=1750`
        * ~~Temperature at startup (w/heatsinks, no top on case (no fan)): ?~~
        * ~~Octane 2.0 score: ~ 8800~~
    * 1.9 GHz (Recommended based on analysis linked above...keeps power consumption < 10 W)
        * `over_voltage=4`
        * `arm_freq=1900`
        * ~~Temperature (w/heatsinks, no top on case (no fan)): ~~
            * ~~At startup: ~ 60 deg. C~~
            * ~~During CPU benchmarks: ~ 65 deg. C~~
        * Octane 2.0 score: 10470
    * 2.0 GHz with:
        * `over_voltage=6`
        * `arm_freq=2000`
        * Temperature (w/heatsinks, no top on case (no fan))
            * At startup: ~ 52 deg. C
            * During stress (Octane): ~ 58 deg. C
        * Octane 2.0 score: 10987
1. Raspberry Pi OS:
    1. Set the desired frequency and voltage in config.txt: `sudo nano /boot/config.txt`
    1. Reboot for changes to take effect: `sudo reboot`
1. Ubuntu 20:
    * Set the desired frequency and voltage as above, except in `/boot/usercfg.txt`
        * While Ubuntu is running, the file is in `/boot/firmware/usercf.txt`

## Troubleshooting
* No Audio
    * Right click the Speaker icon in the top right corner and select `Analog`
* Crackly Audio
    * Turn down volume *in the software* (apparently high volume settings attempt to use hardware amplification on Raspberry Pi which is not great)
* `Waiting for cache lock: Could not get lock /var/lib/dpkg/lock...` error message
    * This is most likely due to unattended-upgrades. Wait for this process to finish before proceeding...
    * DO NOT use: `sudo rm /var/lib/dpkg/lock-frontend` (or `.../dpkg/lock`) as this may crash the system
* Blank monitor after startup (NOOBS)
    * Hold shift at startup for Recover mode
    * Click `Edit config (e)`
    * Modify config.txt as follows:
        * Uncomment `config_hdmi_boost=4`
        * UPDATE: Apparently the above line is ignored by Raspberry Pi 4 ([see here](https://www.raspberrypi.org/documentation/configuration/config-txt/video.md))
* Connect to hidden WiFi on Raspberry Pi OS
    * [Reference](https://www.raspberrypi.org/forums/viewtopic.php?t=250588)
    1. Edit `/etc/wpa_supplicant/wpa_supplicant.conf`
        * Add `scan_ssid=1` under `network`
        * e.g. 
        ```bash
        network={
            ssid="your hidden SSID"
            scan_ssid=1
            psk="your password"
            key_mgmt=WPA-PSK
        }
        ```

## Configure Boot from USB Mass Storage Device (MSD) (Beta...doesn't work :( )
* [Reference](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md#usbmassstorageboot)
    * Scroll down to `USB Mass Storage` section
### Update the Bootloader
1. Boot into a standard Raspberry Pi OS install (on SD card)
1. Modify `/etc/default/rpi-eeprom-update` (need `sudo`)
    * Change `FIRMWARE_RELEASE_STATUS="critical"` to `..."stable"`
1. Install the stable version of the bootloader and enable USB boot
    * `sudo rpi-eeprom-update -d -f /lib/firmware/raspberrypi/bootloader/stable/pieeprom-2020-06-15.bin`
1. Reboot: `sudo shutdown -r now`
1. Check the bootloader version and config:
    ```bash
    vcgencmd bootloader_version 
    vcgencmd bootloader_config
    ```
    * NOTE: `BOOT_ORDER=0xf41` means try booting from SD, then USB, repeat indefinitely

### Create a bootable USB drive w/Ubuntu 18.04
1. Download Ubuntu Mate for Raspberry Pi 4 from [here](https://ubuntu-mate.org/download/arm64/bionic/)
    * [Direct download](https://ubuntu-mate.org/raspberry-pi/ubuntu-mate-18.04.2-beta1-desktop-arm64+raspi3-ext4.img.xz)
1. Flash it onto a blank USB device with Etcher
1. Update the GPU firmware
    * OPTION 1:
        * Boot into Raspberry Pi OS
        * Run `sudo apt update`, `sudo apt full-upgrade`, and then `sudo rpi-update` to update the firmware on the SD card install
        * Copy the `*.elf` and `*.dat` files from `/boot` into the `system-boot` partition on the bootable USB drive
            ```bash
            cd /boot
            sudo cp *.elf /media/pi/system-boot
            sudo cp *.dat /media/pi/system-boot
            ````
            * NOTE: From this point on, `sudo rpi-update` can be used from within the OS on the USB boot device.
    * OPTION 2:
        * Copy the `*.elf` and `*.dat` files from the [Raspberry Pi Firmware Github](https://github.com/raspberrypi/firmware/tree/master/boot)
1. Shutdown (`sudo poweroff`)
1. Remove SD card, and insert bootable USB w/Ubuntu image
1. Power on and boot from USB
