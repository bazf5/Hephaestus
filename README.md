Setup

Enable SSH
1. Run sudo raspi-config
2. Enable SSH
3. Change hostname
Now, the rest of the steps can be completed by SSH'ing into the box

Create a new user
1. SSH into Pi
2. Create the new user: sudo useradd hon -s /bin/bash -m -G adm,sudo
3. Set the password to something complete: sudo passwd hon
4. Create any other user accounts you may need
5. Run sudo nano /etc/sudoers and set: sudo ALL=(ALL) NOPASSWD: ALL
6. Log out and log back in as hon
7. Remove default pi account & directory: sudo userdel pi && sudo rm -rf /home/pi

Disable SSH password authentication
1. If you haven't already, create an SSH key on the remote computer with ssh-keygen
2. Copy key to hon account on the Pi: ssh-copy-id hon@HOSTNAME
3. SSH into the Pi and disable password authentication: echo "PasswordAuthentication no" | sudo tee -a /etc/ssh/sshd_config

Change default settings

Harden security on system
1. Follow steps at https://www.raspberrypi.org/documentation/configuration/security.md

Disable HDMI CEC & Set GPU Memory
1. echo "hdmi_ignore_cec=1" | sudo tee -a /boot/config.txt
2. echo "hdmi_ignore_cec_init=1" | sudo tee -a /boot/config.txt
3. echo "gpu_mem=16" | sudo tee -a /boot/config.txt

Disable screen saver
Edit /etc/kbd/config and set:
1. BLANK_TIME=0
2. BLANK_DPMS=off
3. POWERDOWN_TIME=0
Source: https://www.raspberrypi.org/documentation/configuration/screensaver.md

Disable screen blanking
1. Disable screen blanking by editing /boot/cmdline.txt and adding: consoleblank=0

Update settings in raspi-config
* Expand file system
* Change locale, keyboard layout and timezone
* Force audio through 3.5mm
Note: A reboot will be required now.

Update/Upgrade system
1. sudo apt-get -y update && sudo apt-get -y upgrade

Install Required Packages
Required:
1. sudo apt-get -y install git git-core build-essentials
2. sudo apt-get -y install lynx netatalk libssl-dev
3. sudo apt-get -y install alsa-utils mpg321 mplayer
4. sudo apt-get -y install bluetooth bluez libbluetooth-dev libudev-dev
5. sudo apt-get -y install libavahi-compat-libdnssd-dev
6. sudo apt-get -y install libcap2-bin libtool-bin
7. sudo apt-get -y install python-setuptools python-dev python-rpi.gpio
8. sudo apt-get -y install android-tools-adb
9. sudo apt-get -y install sshfs

Install Node via nvm
Follow instructions at https://github.com/creationix/nvm or:
1. Follow instructions at https://github.com/nvm-sh/nvm#installing-and-updating
2. source ./.bashrc
3. nvm install 6

Install global node modules
1. npm install -g forever

Setup Bluetooth
1. sudo apt-get -y install bluetooth bluez libbluetooth-dev libudev-dev
2. find -path '*noble*Release/hci-ble' -exec sudo setcap cap_net_raw+eip '{}' \;
3. sudo setcap cap_net_raw+eip $(eval readlink -f `which node`)

Setup Flic
Follow instructions at https://community.home-assistant.io/t/install-flic/16969/4

Clone Repo
1. git clone https://github.com/petele/HomeOnNode.git
2. cd HomeOnNode
3. ./setup.sh
4. Update app/Keys.js
5. cp login.sh ~

Set up log rotation
Edit /etc/logrotate.conf and add:
"/users/pi/HomeOnNode/app/logs/rpi-system.log" {
  rotate 4
  weekly
  missingok
  nocompress
}

Copy dotfiles
* Copy dotfiles

Set login.sh to run automatically
1. Edit ~/login.sh and have it start whatever is necessary.
2. Edit .bashrc and add ./login.sh to the bottom of the file.
Celebrate!

Other notes and resources

Interesting projects
* HomeAssistant
* Node Sonos

Harmony Info
* Protocol Guide 1
* Protocol Guide 2

No longer used

Install Z-Wave
1. Install USB stuff for z-wave sudo apt-get -y install libcap2-bin libudev-dev libusb-1.0-0-dev libpcap-dev
2. Follow instructions from OpenZWaveShared
3. sudo ldconfig
4. git clone https://github.com/OpenZWave/open-zwave
