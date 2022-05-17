# Raspberry Pi OS True tty (console)

I use the serial console to run Raspberry Pi boards nearly all of the time when they're first being setup. I very rarely ever connect a Pi to a screen or keyboard. Having the current recipie for using the serial console is invaluable to my workflow and use cases. This repository is a simple set of notes and details to remind me how to setup a Raspberry Pi (raspbian) OS system to a usable state on the console.


### Enable the serial console:

edit /boot/config.txt

add this line:

enable_uart=1

NOTE: If the Pi is a 3, you have to also fix the core CPU clock or the serial connection baud rate won't be correct. For a Pi 3, you add this:

edit /boot/config.txt

add this line: 

core_freq=250

### Default user

As of a 2022 Raspberry Pi OS (raspbian was such a better name for searching, rip), the 'pi' default user is no longer on the OS image for security/brute force reasons. They're good reasons for the change. I just wish there was a better documented system to create a default user. After much searching I've got my recipie:

create a file: /boot/userconf

Quoting raspberrypi.com / Simon Long:

There are also mechanisms to preconfigure an image without using Imager. To set up a user on first boot and bypass the wizard completely, create a file called userconf or userconf.txt in the boot partition of the SD card; this is the part of the SD card which can be seen when it is mounted in a Windows or MacOS computer.

This file should contain a single line of text, consisting of username:encrypted-password – so your desired username, followed immediately by a colon, followed immediately by an encrypted representation of the password you want to use.

To generate the encrypted password, the easiest way is to use OpenSSL on a Raspberry Pi that is already running – open a terminal window and enter

echo 'mypassword' | openssl passwd -6 -stdin

This will produce what looks like a string of random characters, which is actually an encrypted version of the supplied password (mypassword).

For example, the single line with a user 'crandall' and password 'change2022' would be this:

crandall:$6$1JGNeDIDxzSXxPZB$sROxz8KfO2G2.AecbsPNQuSNaAOw9PbU9jEd3Ew3Yex73UhVesZlka4AJpFIEw6bfH6ahu/Q21lGI/QVW/RRI.

The crandall user will be setup as the default login, just like the old pi user was. It'll have sudo and access to the various /dev/ subsystems. At this point, you can boot the pi, drop onto the tty console and login without any SSH or networking required.

Source for Raspberry Pi Bullseye post April 2022:
https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/
