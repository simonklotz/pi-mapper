# My ofxPiMapper - Raspberry Pi 5 installation docs
I tried to get the [ofxPiMapper](https://ofxpimapper.com/) running on a 8GB Raspberry Pi 5.\
This is my documentation.

Same steps will work on a PI 4 B, as [@ali-layken](https://github.com/ali-layken) [confirmed](https://github.com/kr15h/ofxPiMapper/issues/169). But I didn't try.\
Btw thanks for his tip, to use the addons.make.norpi!

## First attempt: desktop
I used Raspberry Pi Imager v1.8.5 to load the Raspberry Pi OS (64-bit) onto my sd card.\
(Debian Bookworm with the Raspberry Pi Desktop. Published: 2024-11-19)

### openFrameworks installation
I tried to follow the outdated [raspberry-pi-getting-started instructions](https://openframeworks.cc/setup/raspberrypi/raspberry-pi-getting-started/):

#### Configure the Raspberry Pi
```bash
  $ sudo raspi-config
```
 - **Memory Split**: is no longer listed in the config tool.\
   I googled and found out that I could also set it via /boot/firmware/config.txt but as I use a modern (8GB) Pi 5\
   I skipped this step with the hope, that this setting was only necessary for the older PIs with less memory.
 - **Boot Options**: is now accessible under "1 System Options > S5 Boot / Auto Login":\
   I selected "B4 Desktop Autologin"
 - **"new experimental" GL Driver**: I couldn't find this option anymore. I also skipped this step with the assumption\
   that this is not experimental anymore and now used by default.
 - **Xorg**: this is not a step in the raspberry-pi-getting-started, but I wanted to be sure that I use Xorg and not Wayland.
   So I selected "6 Advanced Options > A6 Wayland > W1 X11".
```bash
  $ sudo apt update
  $ sudo apt upgrade
  $ uname -a
  Linux pimapper 6.6.51+rpt-rpi-2712 #1 SMP PREEMPT Debian 1:6.6.51-1+rpt3 (2024-10-08) aarch64 GNU/Linux
  $ sudo reboot
```

#### Download openFrameworks
 - I downloaded [openFrameworks linux arm64](https://github.com/openframeworks/openFrameworks/releases/download/0.12.0/of_v0.12.0_linuxaarch64_release.tar.gz) and extracted it into the folder /home/pi/openFrameworks

#### Install packages and compile openFrameworks
```bash
  $ cd /home/pi/openFrameworks/scripts/linux/debian
  $ sudo ./install_dependencies.sh
  $ make Release -C /home/pi/openFrameworks/libs/openFrameworksCompiled/project
```

#### Compile your first app
 - I tried out one of the examples like it's suggested in the docs to see if ofx works:
```bash
  $ cd /home/pi/openFrameworks/examples/graphics/polygonExample
  $ make
  $ make run
```

### ofxPiMapper
I downloaded and moved the [repository](https://github.com/kr15h/ofxPiMapper/tree/master) into /home/pi/openFrameworks/addons/ofxPiMapper.
In an earlier try I realized that the ofxOMXPlayer doesn't work with RPI4/5. So I tried without:
```bash
  $ cd /home/pi/openFrameworks/addons/ofxPiMapper/example_basic
  $ mv addons.make addons.make.bak
  $ mv addons.make.norpi addons.make
  $ make
```
And it works!
```bash
  $ cd bin
  $ ./example_basic
```
