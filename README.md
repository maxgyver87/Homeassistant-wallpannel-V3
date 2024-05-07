# Touch Screen
This is my x attempt of a home control panel using raspbian Bullseye, chromium and openbox

## Used Hardware:
- [Raspberry Pi 4 8GB](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/)
- [Raspberry Pi 4 Case (With Cooling Fan) (v3.0)](https://www.thepihut.com/products/raspberry-pi-4-case-with-cooling-fan?variant=31907267706942)
- [Raspberry Pi 15.3W USB-C Power Supply](https://www.raspberrypi.org/products/type-c-power-supply/)
- [UPERFECT y Portable Monitor With FHD 1080P Touch Screen](https://www.aliexpress.com/item/1005002828837342.html?spm=a2g0s.9042311.0.0.4b304c4dO7abT1)

# How-To
- use raspberry pi imager
- select os in my case: Raspberry Pi OS Bullseye (64-bit) Lite
- shift+ctl+x
- check diasble overscan
- set hostname
- enable ssh
- configure WIFI
- set locale settings
- Flash 

# Homeassistant-wallpannel-V3
find ip and ssh to it
```
sudo raspi-config
```
System Options
Boot / Auto Login
Console Autologin
Display Options
Underscan
Yes
/ OK

#Install nessecary software:
```
sudo apt-get install --no-install-recommends xserver-xorg x11-xserver-utils xinit openbox chromium-browser -y
```
# Define autostart
replace: url with yours
```
sudo nano /etc/xdg/openbox/autostart
```
```
xset -dpms
xset s noblank
xset s off
sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/'Local State'
sed -i 's/"exited_cleanly":false/"exited_cleanly":true/; s/"exit_type":"[^"]\+"/"exit_type":"Normal"/' ~/.config/chromium/Default/Preferences
/usr/bin/chromium-browser --kiosk --enable-features=OverlayScrollbar --pull-to-refresh=2  --disable-cache --disk-cache-dir=/dev/null --disk-cache-size=1 --noerrdialogs http://ha-main.local:8123/kiosk-tablet/home
```
## Possible Chromium Flags can be viewed at
- chrome://flags
- https://peter.sh/experiments/chromium-command-line-switches/
# Required
- kiosk
- enable-features=OverlayScrollbar
- noerrdialogs
# Optional
- disable-cache
- disk-cache-dir=/dev/null
- disk-cache-size=1
# Nice to have
- pull-to-refresh=2



# Create Bash profile to define display and hide cursor
```
touch ~/.bash_profile
```
```
sudo nano ~/.bash_profile
```
```
[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx -- -nocursor
```
```
source ~/.bash_profile
```
```
sudo reboot
```
# Rotation Of the Touch input
Navigate to
```
sudo nano /usr/share/X11/xorg.conf.d/40-libinput.conf
```
Find the "touchpad catchall"
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        .......
```
Select rotation<br/>
0 degrees of rotation parameters: ```Option "CalibrationMatrix" "1 0 0 0 1 0 0 0 1"```<br/>
90 degrees of rotation parameters: ```Option "CalibrationMatrix" "0 1 0 0-1 1 0 0 1"```<br/>
180 degrees of rotation parameters: ```Option "CalibrationMatrix" "1 0 0-1 1 0 0 1"```<br/>
270 degrees of rotation parameters: ```Option "CalibrationMatrix" "0-1 1 1 0 0 0 0 1"```<br/>
and paste like:
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchscreen "on"
        Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```
# Set Screen resolution in this case (1080p) 60 Hz
Find your hdmi group and mode [Raspberry Pi  User Guide - 2016 - Upton - HDMI Display Modes.pdf](https://github.com/maxgyver87/Homeassistant-wallpannel-V3/files/15239152/Raspberry.Pi.User.Guide.-.2016.-.Upton.-.HDMI.Display.Modes.pdf)

```
sudo nano /boot/config.txt
```
```
hdmi_group=2
hdmi_mode=82
```
##[Control Display via MQTT](https://blog.cavelab.dev/2021/10/control-rpi-hdmi-with-mqtt)





## UI
## UI Inspiration
[![IMAGE ALT TEXT](http://img.youtube.com/vi/m34MIuhRK1g/0.jpg)](https://youtu.be/m34MIuhRK1g "Room Overview Lovelace Dashboard")
[Will Surridge Tec](https://www.youtube.com/watch?v=m34MIuhRK1g)
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
![qiz-li](https://github.com/qiz-li/lovelace-soft-ui/blob/main/images/screenshot.png)
[qiz-li](https://github.com/qiz-li/lovelace-soft-ui)
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
![danieljarhult](https://community-assets.home-assistant.io/original/3X/2/4/242e1d5f69f5484f36c48def540b4f52c27e336e.jpeg)
[danieljarhult](https://community.home-assistant.io/t/neon-lovelace-ui-and-theme-for-tablets/307230)
[Neon Icons](https://www.flaticon.com/authors/kiranshastry/gradient/4)
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
![matt8707](https://raw.githubusercontent.com/matt8707/hass-config/master/www/img/screenshot.png)
[matt8707](https://github.com/matt8707/hass-config)
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
![Tobiasn2005](https://raw.githubusercontent.com/Tobiasn2005/Home-Assistant/main/www/ui/floorplan1.gif)
[Tobiasn2005](https://github.com/Tobiasn2005/Home-Assistant)
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
![HA_Scott](https://i.redd.it/nr14d6pk4im41.jpg)
[HA_Scott](https://www.reddit.com/user/HA_Scott/)
<br/>
<br/>
- Concepualise the UI helpful - [micro](https://miro.com)
![Brainstorming](/Brainstorming.png)
- [Figma](https://www.figma.com) help [video](https://www.youtube.com/watch?v=n8hbHBgsiEI&t=428s)






### Custom cards needed:
- [custom layout card](https://www.github.com/thomasloven/lovelace-layout-card) help with [layoutit-grid](https://github.com/Leniolabs/layoutit-grid)
- [markdown card](https://www.home-assistant.io/lovelace/markdown/) for side bar with [markdown help](https://guides.github.com/features/mastering-markdown/#examples)
- [custom kiosk mode](https://github.com/maykar/kiosk-mode)
- [custom swipe card](https://github.com/bramkragten/swipe-card)
- [hass browser mod](https://github.com/thomasloven/hass-browser_mod)
- [lovelace card mod](https://github.com/thomasloven/lovelace-card-mod) with [CSS help](https://www.w3schools.com/css/css_font_size.asp)
- [clock](https://github.com/shbatm/mm2-clock-card)
- [Sidebar card](https://github.com/DBuit/sidebar-card)
- [custom button card](https://github.com/custom-cards/button-card)
- [calender](https://github.com/marksie1988/atomic-calendar-revive)
- [Homekit panel card](https://github.com/DBuit/Homekit-panel-card)
- [simple thermostat](https://github.com/nervetattoo/simple-thermostat)
- [serch card](https://github.com/postlund/search-card)
- [banner card](https://github.com/nervetattoo/banner-card)
- [stack in card](https://github.com/custom-cards/stack-in-card)
