+++
title = 'OctoPrint'
date = 2025-05-31T03:23:45+01:00
draft = false
[cascade]
	type = 'blog'
+++

The no fluff super simplified version of how I installed
OctoPrint on a Pi 3B running Raspbian (Debian 12 Bookworm)

Raspberry Pi Imager is used to flash the SD card with PiOS lite
as we don't need a desktop environment, make sure to setup the
network settings unless a LAN cable will be used and **enable
SSH**.

Using the terminal of your choice ssh into the pi

## Installation

Python3 should be pre-installed with Raspbian check that with:
```
python --version
```
It is recommended to install octoprint within a virtual environment
to prevent possible conflics or something:

```
cd ~
sudo apt update
sudo apt install python3 python3-pip python3-dev python3-setuptools python3-venv git libyaml-dev build-essential libffi-dev libssl-dev
mkdir OctoPrint && cd OctoPrint
python3 -m venv venv
source venv/bin/activate
```
Once installed and within the virtual environment use pip to
install OctoPrint:
```
pip install --upgrade pip wheel
pip install octoprint
```
To exit the virtual environment enter type `deactivate`.

Add the user 'pi' (change this to whatever your username
on the pi is) to tty and dialout group so the serial port
(also maybe usb) works:
```
sudo usermod -a -G tty pi
sudo usermod -a -G dialout pi
```

Time to test if the install worked:
```
~/OctoPrint/venv/bin/octoprint serve
```
This will host a website at `http://<pi's ip:5000>` but it is no
good to have to ssh into the pi each time it boots to run this command.

## Automatic start up

Use the text editor of your choice to create the file
`/etc/systemd/system/octoprint.service` and place the following
making sure to **note that your username may not be pi**

```markdown {filename=octoprint.service, hl_lines=[10,11]}
[Unit]
Description=The snappy web interface for your 3D printer
After=network-online.target
Wants=network-online.target

[Service]
Environment="LC_ALL=C.UTF-8"
Environment="LANG=C.UTF-8"
Type=exec
User=pi
ExecStart=/home/pi/OctoPrint/venv/bin/octoprint serve

[Install]
WantedBy=multi-user.target
```

Make this service run on boot by enabling it
```
sudo systemctl enable octoprint.service
```
This serice can now be started/stopped by using
```
sudo serice octoprint {start|stop|restart}
```

## Restart/Shutdown in OctoPrint's web interface

In the UI, under Settings > Server > Commands, configure the following commands:
* Restart OctoPrint: `sudo service octoprint restart`
* Restart system: `sudo shutdown -r now`
* Shutdown system: `sudo shutdown -h now`


{{< callout type="info" >}}
  OctoPrint comminity guide [here](https://community.octoprint.org/t/setting-up-octoprint-on-a-raspberry-pi-running-raspberry-pi-os-debian/2337)
{{< /callout >}}
