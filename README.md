# Homeassistant-wallpannel-V3
https://desertbot.io/blog/raspberry-pi-4-touchscreen-kiosk-setup-64-bit-bullseye
Raspberry Pi OS Bullseye (64-bit) Lite
System Options
Boot / Auto Login
Console Autologin
Display Options
Underscan
Yes
/ OK

sudo apt-get install --no-install-recommends xserver-xorg x11-xserver-utils xinit openbox -y

sudo apt-get install --no-install-recommends chromium-browser -y

sudo nano /etc/xdg/openbox/autostart

xset -dpms
xset s noblank
xset s off
sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/'Local State'
sed -i 's/"exited_cleanly":false/"exited_cleanly":true/; s/"exit_type":"[^"]\+"/"exit_type":"Normal"/' ~/.config/chromium/Default/Preferences
/usr/bin/chromium-browser --kiosk --enable-features=OverlayScrollbar --pull-to-refresh=2  --disable-cache --disk-cache-dir=/dev/null --disk-cache-size=1 --noerrdialogs http://ha-main.local:8123/kiosk-tablet/home

#Possible Flags
- kiosk
- pull-to-refresh=2
- noerrdialogs
- disable-cache
- disk-cache-dir=/dev/null
- disk-cache-size=1
- enable-features=OverlayScrollbar
- touch-events





touch ~/.bash_profile
sudo nano ~/.bash_profile
[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx -- -nocursor
source ~/.bash_profile
sudo reboot

sudo nano /usr/share/X11/xorg.conf.d/40-libinput.conf
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchscreen "on"
        Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection

sudo nano /boot/config.txt
hdmi_group=2
hdmi_mode=82

