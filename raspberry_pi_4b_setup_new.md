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

## Install Ubuntu 20.04 LTS using Etcher
* [Reference/Tutorial](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview)
* [Certified Support for Raspberry Pi in Ubuntu 20](https://ubuntu.com/blog/ubuntu-20-04-lts-is-certified-for-the-raspberry-pi)
1. Download OS image from [here](https://ubuntu.com/download/raspberry-pi):
    * [Ubuntu 20.04.1 LTS (64-bit RPi version)](https://ubuntu.com/download/raspberry-pi/thank-you?version=20.04.1&architecture=server-arm64+raspi)
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


## Install Ubuntu 18.04.5 LTS using Etcher
* [Reference/Tutorial](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview)
* [Support for Raspberry Pi with version 18.04.4 and beyond](https://www.raspberrypi.org/forums/viewtopic.php?t=265415)
1. Download OS image from [here](https://wiki.ubuntu.com/ARM/RaspberryPi)
    * [Ubuntu 18.04.5 LTS (ARM 64 for Raspberry Pi)](http://cdimage.ubuntu.com/releases/bionic/release/ubuntu-18.04.5-preinstalled-server-arm64+raspi3.img.xz)
    * NOTE: [image from here](https://cdimage.ubuntu.com/releases/18.04/release/) did not work
        * [Ubuntu 18.04.5 LTS (64-bit ARM version)](https://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.5-server-arm64.iso)
1. ~~Erase/Format SD card as FAT32 if needed~~ Not needed w/Etcher
1. Use balenaEtcher to flash OS image onto SD card
1. Eject card and reinsert to computer to set up overclocking (see below)
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


## Install & Configure Chrony
* [Reference](https://opensource.com/article/18/12/manage-ntp-chrony)
* [chrony.conf documentation](https://chrony.tuxfamily.org/doc/3.4/chrony.conf.html)
1. `sudo apt install chrony`
1. Modify `/etc/chrony/chrony.conf` to include the following
    * `makestep 1 -1` (step instead of slew for discrepancies > 1 sec.)
    * To sync to a local server, add the following lines under the pool ntp.ubuntu...etc.
      * `server 192.168.1.xxx iburst offline prefer`
      * `allow 192.168.1` (or 10.0.1, etc. whatever the router's LAN addresses look like)
    * For the machine acting as server:
        * `local stratum 10`
    
1. Monitoring/Troubleshooting
    * `chronyc source` shows the status of each server
        * Use the `-v` option for description of symbols (e.g. `chronyc sources -v`)
    * `watch chronyc sources` continuously refreshes output
    * After changing `chrony.conf`, chrony can be reset to apply changes
        * `sudo systemctl restart chrony`

## Enable Automatic Login (Ubuntu 18 & 20)
* [Reference](https://techpiezo.com/linux/enable-or-disable-automatic-login-in-ubuntu-20-04/)
1. Click `Menu` at top left and select `Control Center`
1. In `Users and Groups`, click `Change...` next to: `Password: Asked on login`
1. Check the box that says `Don't ask for password on login`, then click `Ok`
1. Modify `/etc/lightdm/lightdm.conf`
    * If the file doesn't exist, create it: `sudo nano /etc/lightdm/lightdm.conf`
    * Append the following to the file:
    ```bash
    [Seat:*]
    autologin-user=ubuntu
    autologin-user-timeout=0
    ```

## Disable Bluetooth (Ubuntu 18 & 20)
1. `sudo nano /etc/rc.local`
    * Make sure file starts with `#!/bin/sh -e`
    * Add `rfkill block bluetooth` before the `exit 0` line
    * Make sure file ends with `exit 0`
    * Make sure file is executable: `sudo chmod +x /etc/rc.local`

## Install ROS 1 Melodic (Ubuntu 18)
1. Follow the [standard instructions for Ubuntu](http://wiki.ros.org/melodic/Installation/Ubuntu)
    * Use `sudo apt install ros-melodic-desktop`
1. Install additional packages as needed:
    * MAVROS 
        * `sudo apt install ros-melodic-mavros*`
        * After this is installed, download the [GeographicLib dataset install script](https://github.com/mavlink/mavros/blob/master/mavros/scripts/install_geographiclib_datasets.sh)
        * Make the file executable and run it in a terminal: `sudo ./install_geographiclib_datasets.sh`
        * Test mavros with: `roslaunch mavros px4.launch fcu_url:=/dev/ttyPixhawk`
    * ROS Serial
        * `sudo apt install ros-melodic-rosserial*`

## Install ROS 1 Noetic (Ubuntu 20)
1. Follow the [standard instructions for Ubuntu](http://wiki.ros.org/noetic/Installation/Ubuntu)
1. Install additional packages as needed:
    * MAVROS 
        * `sudo apt install ~nros-noetic-mavros.*`
        * After this is installed, download the [GeographicLib dataset install script](https://github.com/mavlink/mavros/blob/master/mavros/scripts/install_geographiclib_datasets.sh)
        * Make the file executable and run it in a terminal: `sudo ./install_geographiclib_datasets.sh`
        * Test mavros with: `roslaunch mavros px4.launch fcu_url:=/dev/ttyPixhawk`
    * ROS Serial
        * rosserial debians have not been released for Noetic yet, so it must be built from source
        * Download the source code from [https://github.com/ros-drivers/rosserial](https://github.com/ros-drivers/rosserial) as a zip file
            *  Make sure to select `noetic-devel` branch (should be the default)
        * Exract the zip file and remove all the subdirectories of `rosserial-noetic-devel` except for `rosserial` and `rosserial_python`
        * Place the rosserial-noetic-devel folder in the FAM workspace to be built
        * Build as usual with e.g. `catkin_make`

## Install ROS 2 Foxy (Ubuntu 20)
1. Follow the [standard instructions for Ubuntu/debians](https://index.ros.org/doc/ros2/Installation/Foxy/Linux-Install-Debians/)

## Set up TightVNC Server for Headless Development
* References
    * [DigitalOcean VNC tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-20-04)
1. Install tightvncserver: `sudo apt install tightvncserver`
1. Run `tightvncserver` in a terminal for initial setup
    * Set the password to access remote desktops
    * There is no need to set a view-only password
    * This sets up a startup script so tightvncserver automatically starts running at boot up.01.
1. Modify the xstartup file to fix prevent 'could not acquire name on session bus' error message
    * `nano ~/.vnc/xstartup`
    * Add: `unset DBUS_SESSION_BUS_ADDRESS`
    * Change `/etc/X11/XSession` to `mate-session`
    * Kill the currently running server: `vncserver -kill :1`
    * Start a new server with `tightvncserver`
1. Install TightVNC Java Viewer from the [TightVNC download page](https://www.tightvnc.com/download.php) to connect from remote computer
    * [Java Viewer](https://www.tightvnc.com/download/2.8.3/tvnjviewer-2.8.3-bin-gnugpl.zip)
    * Extract the folder and make the `.jar` file executable
1. Run the Java Viewer
    * Type in the IP address of the remote device
    * Set the port to 5900 + the display number (default display is :1, so port 5901)
1. Miscellaneous notes
    * To kill a tightvncserver process on the remote machine, use `vncserver -kill :1`
    * Try using xfce4 Desktop environment (maybe more lightweight than MATE?)
        * `sudo apt install xfce4`
        * modify xstart to `xfce4-session` instead of `mate-session`

## Overclocking
* [Reference](https://magpi.raspberrypi.org/articles/how-to-overclock-raspberry-pi-4)
* [Thorough analysis of different speeds and temperatures](https://qengineering.eu/overclocking-the-raspberry-pi-4.html)
1. Check the current CPU speed
    * One time: `vcgencmd measure_clock arm`
    * Monitor: `watch -n 1 vcgencmd measure_clock arm`
1. Set 2.0 GHz with:
    * `over_voltage=6`
    * `arm_freq=2000`
    * Temperature (w/heatsinks, no top on case (no fan))
        * At startup: ~ 52 deg. C
        * During stress (Octane): ~ 58 deg. C
    * Octane 2.0 score: 10987
1. Ubuntu 20:
    * Set the desired frequency and voltage as above, except in `/system-boot/usercfg.txt`
        * While Ubuntu is running, the file is in `/boot/firmware/usercfg.txt`
1. Ubuntu 18:
    * Set the desired frequency and voltage in `/writeable/boot/firmware/config.txt`

## Install Adafruit Python libraries
* [Reference](https://gpiozero.readthedocs.io/en/stable/installing.html)
* [Raspberry Pi GPIO Usage](https://www.raspberrypi.org/documentation/usage/gpio/)
1. Update Raspberry Pi and Python
   * `sudo apt update`
   * `sudo pip3 install --upgrade setuptools`
1. Install GPIOZero Python Libraries
   * `sudo apt install python3-gpiozero`

## Miscellaneous Enhancements
1. Create bash function to generate udev rules for symlinks
    * Open ~/.bashrc and add the following function at the bottom:
    ```bash
    # Make a udev rule entry for a SYMLINK
    #  INPUT: 1) Device name (e.g. /dev/ttyACM0)
    #         2) Desired SYMLINK (e.g. ttyPixhawk)
    function generate_udev_rule()
    {
      vend=`udevadm info -a -n $1 | grep -m1 '{idVendor}' | sed -r -e 's/^\s+//' -e 's/\s+//'`
      prod=`udevadm info -a -n $1 | grep -m1 '{idProduct}' | sed -r -e 's/^\s+//' -e 's/\s+//'`
      serial=`udevadm info -a -n $1 | grep -m1 '{serial}' | sed -r -e 's/^\s+//' -e 's/\s+//'`
      subsys=`udevadm info -a -n $1 | grep -m1 'SUBSYSTEM==' | sed -r -e 's/^\s+//' -e 's/\s+//'`
      echo "$subsys, $vend, $prod, $serial, SYMLINK+=\"$2\""
    }
    ```
    * Use `ls /dev/tty*` to show available devices to use as input to the above
    * Place the output into a file in `/etc/udev/rules.d`
        * e.g. `99-usb-serial.rules`
    * udev rules can be reloaded with: `sudo udevadm control --reload-rules
1. Display CPU temperature in top panel
    * [Reference](https://www.raspberrypi.org/forums/viewtopic.php?t=252115)
    1. Create a bash script on Desktop (`cpu_temp.sh`) with the following contents
    ```bash
    #!/bin/bash
    temp=$(</sys/class/thermal/thermal_zone0/temp)

    temp_f=`echo "$temp/1000" | bc -l`
    printf "CPU Temp: %.3f°C\n"  $temp_f
    ```
    1. Make the script executable: `chmod +x cpu_temp.sh`
    1. Right click top panel and select `Add to panel`
    1. Select `Command`
        * Right click the time that appeared in the panel and click `Preferences`
        * For Command, list the path to the script: `/home/ubuntu/Desktop/cpu_temp.sh`
        * Set the interval as desired (e.g. 5 seconds)

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
* Resolve `System program problem detected`
    * [Reference](https://itsfoss.com/how-to-fix-system-program-problem-detected-ubuntu/)
    * Stop the persistent warning by deleting the crash reports
        * `sudo rm /var/crash/*`
* Fix HDMI only allowing 640x480 resolution (in `/boot/firmware/usercfg.txt`)
    * [Reference](https://askubuntu.com/questions/1215775/arm-ubuntu-wont-let-me-change-my-resolution)
    * [Raspberry Pi Video settings](https://www.raspberrypi.org/documentation/configuration/config-txt/video.md)
    * Set hdmi_group for Display Monitor Timings (DMT, i.e. monitors vs. TVs)
        * `hdmi_group=2`
    * Set Normal HDMI mode (i.e. Not DVI)
        * `hdmi_drive=2`
    * Configure 1920x1200 @ 60 Hz
        * `hdmi_mode=68`
    * Example usercfg.txt (HDMI:0 refers to HDMI port closest to USB-C power):
        ```bash
        [HDMI:0]
        hdmi_group=2
        hdmi_drive=2
        hdmi_mode=68
        ```
        
## Backing up the SD card
* [Reference](https://www.raspberrypi.org/documentation/linux/filesystem/backup.md)
* [See also](https://beebom.com/how-clone-raspberry-pi-sd-card-windows-linux-macos/)
* [And this](https://stackoverflow.com/questions/19355036/how-to-create-an-img-image-of-a-disc-sd-card-without-including-free-space)
1. Insert the SD card into a computer
1. Use `fdisk -l` or `df -h` or the `Disks` App to get the SD card device name
1. Copy the entire SD image to a file: 
    * WARNING: Be very careful to get the `of` parameter correct -- `dd` can easily overwrite your hard drive!
    * `sudo dd bs=4M if=/dev/mmcblk0 of=PiOS.img status=progress`
    * Compressed version: `sudo dd bs=4M if=/dev/mmcblk0 status=progress | gzip > PiOS.img.gz`
1. Restore/overwrite SD card
    * NOTE 1: Try using Etcher first...
    * NOTE 2: You may need to unmount the SD card before running the following commands
        * `sudo mount | grep /dev/mmcblk0` (to see whether the card is mounted)
        * `sudo umount /dev/mmcblk0`
    * `sudo dd bs=4M if=PiOS.img of=/dev/mmcblk0 status=progress`
    * Compressed version: `gunzip --stdout PiOS.img.gz | sudo dd bs=4M of=/dev/mmcblk0 status = progress`
