# octoprint-lcd-theme-black-pearl
An attractive Conky theme for an LCD attached to a 3D printer running OctoPrint

![black-pearl-screencap](https://user-images.githubusercontent.com/15971213/39142326-362ae09a-46df-11e8-83f6-9c0b08b14b2e.png)

## Overview
The stock Robo C2 printer from Robo 3D had a Kivy-based touchscreen interface but I thought I'd mod the 3D printer to make something unique.

> [Black Pearl](https://www.deviantart.com/art/Black-Pearl-Conky-281468275) was the original Conky theme from which this was inspired but was completely changed to accommodate the latest version of Conky itself and the limited resolution of this device

> [Conky](https://github.com/brndnmtthws/conky) is a light-weight system monitor for the X11 graphics system which allows a variety of customization options to allow styling, running on a variety of UNIX flavors

> [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) is the underlying operating system which runs on the Raspberry Pi 3 single-board computer inside the Robo C2 3D printer (and many other upgraded printers)

> [OctoPrint-robotheme](https://github.com/Robo3D/OctoPrint-robotheme) was the plugin formerly installed on the Robo C2 before this mod as well as their private code for the LCD screen itself

> [OctoPrint](https://github.com/foosel/OctoPrint) is the original web interface from which their product and theme were based

## Notes
* The original setup did not include the X11 graphics interface nor a desktop. This design adds a bootup desktop using the X11 graphics interface.
* The installation of an LCD/TFT screen onto a Raspberry Pi 3 is outside of the scope of this README and has been known to be problematic. It is assumed that you have successfully done this before proceeding.
* My TFT is the one with the ads7846 driver which results in 480x320 within the Conky configuration's resolution. If you have a different size, it may be necessary to painstakingly adjust the `${offset}`, `${voffset}` and similar X/Y coordinates as seen in the two involved files.
* There are various ways of starting up Conky but I have streamlined things into one LUA chart file and the default `.conkyrc` configuration file.
* Unlike many public respositories, this one will require that you download the set of files and to place things carefully where they belong. If you're not comfortable using the following commands in a terminal then this probably isn't the mod for you: `cd`, `mkdir`, `ls -a`, `ssh`, `scp`, `sudo`.
* You should have a method of backing up and restoring images to your Raspberry Pi 3's microSD. I would suggest [ApplePi Baker](https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/) which does an excellent job of this. It goes without saying that you should purchase another microSD card and work on a copy of your printer's image rather than the original. If anything goes wrong, you can always revert back to the first.

## Installation
Checklist before starting:

- [x] You've cloned your original microSD and you're beginning with the new card
- [x] You've got a working LCD/TFT screen attached to your Raspberry Pi 3 and you see a login prompt when it boots
- [x] \(Perhaps\) temporarily, you've got a USB-based keyboard and mouse connected to the Raspberry Pi 3 in case you need it
- [x] You can both `ping` and `ssh` to your Raspberry Pi 3 by it's `hostname.local`, for example, `octopi.local`.
- [x] You have run `sudo raspi-config` and have setup the boot option so that it successfully logs in automatically into the graphical desktop each boot, as well as setting the maximum resolution that the LCD can handle and finally, you've set the localization features to include your timezone
- [x] You have used the Desktop Appearance Settings to set the background image to `Desktop -> Layout -> No image` and to disable the Screensaver
- [x] You've right-mouse clicked on the Task Bar and set `Panel Settings -> Advanced -> Automatic Hiding -> Minimize panel when not in use`

### Instructions
1. Remote into the Raspi, substituting for its name here: `ssh pi@octopi.local`
2. `cd ~`
3. `git clone https://github.com/OutsourcedGuru/octoprint-lcd-theme-black-pearl.git`
4. `cd octoprint-lcd-theme-black-pearl`
5. `sudo apt-get install -y conky conky-all curl lm-sensors hddtemp`
6. `mkdir -p ~/.config/autostart`
7. `mkdir -p ~/.conky/lua`
8. `cp .conky/lua/clock.lua ~/.conky/.`
9. `cp .conkyrc ~`
10. Test things first by running `conky` from the Raspberry Pi 3 menu interface. You should see the theme come up on the LCD. If this works, proceed from your remote `ssh` session...
11. `cp ~/octoprint-lcd-theme-black-pearl/.config/autostart/conky.desktop ~/.config/autostart/.`
12. `sudo reboot`

### Details
The configuration itself is in the two files: `~/.conkyrc`, `~/.conky/lua/clock.lua`. Don't confuse these with the source files which you cloned from the repository under the `octoprint-lcd-theme-black-pearl` folder.

The font selection in Conky is sometimes troublesome so "simpler is better". For example, both these seem to work, noting the symbols and delimiters:

```
${font neuropol:size=18}
${font Nimbus Mono L,size:7,style:bold}
```

Trying to reverse this wouldn't necessarily produce what you'd be expecting. If you make changes, make sure to verify what you've tried to do is what actually happened.

A vertical space in the `config.text` section will produce the same on the screen. If you remove any extra hard returns, this will change the formatting. Note that you can separate blocks using an otherwise empty `#` comment, however.

All the clock charts are produced inside the `clock.lua` file. I have organized things to make editing them easier.

In the unlikely event that you're trying to run this from a Raspberry Pi Zero, you'll want to comment out three of the core rings and the corresponding text for those. You'll then need to adjust the `${voffset}` for the following section back in the `.conkyrc` file.

I'm assuming that you're using the embedded wi-fi adapter `wlan0` in the Raspi. If you are using a USB-based wi-fi adapter then you would need to discover the adapter's name using `ifconfig` and then update the two files to replace.