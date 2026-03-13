# pibx

My Raspberry Pi PBX system.

## Hardware
- Raspberry Pi 4 Model B 2GB
- 64GB SD card

## OS
Raspberry Pi OS Lite 64-bit ([trixie](https://www.debian.org/releases/trixie/))\
Installed using [Raspberry Pi Imager](https://www.raspberrypi.com/documentation/computers/getting-started.html#raspberry-pi-imager)

## Setup
```
sudo dpkg-reconfigure locales
sudo rm /etc/motd
sudo apt update && sudo apt full-upgrade
sudo apt install git keychain
```
TODO install mysql (mariadb)


## Install Asterisk
### Download and Configure
```
cd /usr/local/src
sudo wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-22-current.tar.gz
sudo tar xzf asterisk-22-current.tar.gz
cd asterisk-22*/
sudo contrib/scripts/install_prereq install
sudo ./configure
```

### Run Menuselect

```
sudo make menuselect
```
- Select the following:
  - TBD (probably need ulaw and 722 sounds, and extra sounds)

### Build and Install
```
sudo make
sudo make install

# Install sample configuration files
sudo make samples

# Install init scripts for systemd
sudo make config

# Install logrotate configuration
sudo make install-logrotate
```
TODO: may want to remove `make config` and replace with custom config files provided from this project

### Create Asterisk User
```
sudo groupadd asterisk
sudo useradd -r -d /var/lib/asterisk -g asterisk asterisk
sudo chown -R asterisk:asterisk /etc/asterisk
sudo chown -R asterisk:asterisk /var/lib/asterisk
sudo chown -R asterisk:asterisk /var/log/asterisk
sudo chown -R asterisk:asterisk /var/spool/asterisk
sudo chown -R asterisk:asterisk /var/run/asterisk
```

## References
- [Raspberry Pi OS: Update software](https://www.raspberrypi.com/documentation/computers/os.html#update-software)
- [Installing Asterisk From Source](https://docs.asterisk.org/Getting-Started/Installing-Asterisk/Installing-Asterisk-From-Source/)
- [Install Asterisk on Debian 13 (Trixie)](https://www.ipcomms.net/blog/asterisk-debian-13-install/)
- [Codecs Supported by voip.ms](https://wiki.voip.ms/article/Codecs_Supported)
