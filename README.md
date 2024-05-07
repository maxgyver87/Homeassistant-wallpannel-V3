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
## Possible Flags
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
# Set Screen resolution in this case 1080x1920
```
sudo nano /boot/config.txt
```
```
hdmi_group=2
hdmi_mode=82
```
