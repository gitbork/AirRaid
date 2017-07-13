# AirRaid
## Raspberry Pi Civil Defense Siren
Proof of Concept Code to Build a Hackable Realistic Civil Defense Siren. To Demonstarate the vulerability in Millions of Real Civil Defense Sirens that work the same across the country 

[![N|Solid](https://static.wixstatic.com/media/632ed1_fb9a3b5268b3477caea3a865bef54a7c.png/v1/fill/w_119,h_67,al_c,usm_0.50_1.20_0.00/632ed1_fb9a3b5268b3477caea3a865bef54a7c.png)](https://maxwelldps.com)

 Raspberry Pi Civil Defense Siren is designed to emulate Federal Signal Systems DTMF ONLY for Now.

### Hardware Needed
  - SDR
  - Adafruit DRV8871 Brushed DC Motor Driver Breakout - https://learn.adafruit.com/adafruit-drv8871-brushed-dc-motor-driver-breakout/overview 
  - Raspberry Pi Model 1, 2, 3

  - RTL_SDR
  - Multimon-ng
  - wiringPi

### Development

Want to contribute? Great!

Decoding Code Needed For:
- Two-tone sequential
- EAS
- POCSAG
- Digital AFSK 

### Tech

This uses a number of open source projects to work properly:

* [RTL_SDR](https://github.com/keenerd/rtl-sdr) - turns your Realtek RTL2832 based DVB dongle into a SDR receiver.
* [MULTIMON-NG](https://github.com/EliasOenal/multimon-ng) - multimon-ng a fork of multimon. It decodes digital transmission modes.
* [WiringPi](https://github.com/WiringPi/WiringPi) - Gordon's Arduino wiring-like WiringPi Library for the Raspberry Pi (Unofficial Mirror for WiringPi bindings).

And of course Raspberry Pi Civil Defense Siren itself is open source with a Public Repo on GitHub.

### Installation of Needed Software

#### Building From Single Command
```
$ curl -o installer.sh https://github.com/MaxwellDPS/AirRaid && chmod +x installer.sh && sudo ./installer.sh
```
#### Building From Source
```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
sudo apt-get -y install git gcc cmake libusb-1.0-0-dev libpulse-dev libx11-dev screen qt4-qmake libtool autoconf automake libfftw3-dev

mkdir ~/src

echo "Installing rtl_sdr"
cd ~/src
git clone git://git.osmocom.org/rtl-sdr.git
cd ~/src/rtl-sdr
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make
sudo make install
sudo ldconfig

echo "Installing multimon-ng"
cd ~/src
git clone https://github.com/EliasOenal/multimonNG.git
cd ~/src/multimonNG
mkdir build
cd build
qmake ../multimon-ng.pro
make
sudo make install

echo "Installing Kalibrate"
cd ~/src
git clone https://github.com/asdil12/kalibrate-rtl.git
cd kalibrate-rtl
git checkout arm_memory
./bootstrap
./configure
make
sudo make install

echo "Installing WiringPi"
sudo apt-get purge wiringPi -y
hash -r
cd ~/src
git clone git://git.drogon.net/wiringPi
cd ~/wiringPi
git pull origin
cd ~/wiringPi
./build

mkdir siren
cd siren
git clone https://github.com/MaxwellDPS/AirRaid.git
cd Raspberry-Pi-Civil-Defense-Siren
gcc -lpthread -lwiringPi -o rpiSiren siren.c
sudo mkdir /opt/rpiSiren/
sudo mv rpiSiren /opt/rpiSiren/rpiSiren
sudo chmod +x /opt/rpiSiren/rpiSiren
sudo ln -s /opt/rpiSiren/rpiSiren /usr/local/bin
```

#### Running
```
$ rpiServer
```
License
----

Apache 2.0


