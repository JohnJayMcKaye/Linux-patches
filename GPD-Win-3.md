# [GPD WIN 3](https://www.gpd.hk/gpdwin3)
- ## touchscreen
  
  Füge
  ``exclude=kernel*``
  zur Datei/
  ``etc/dnf/dnf.conf``
  hinzu um ein Kernel-5.16 update zu verhindern
  ```
  sudo modprobe -r goodix
  sudo modprobe goodix
  ```
  sudo nano /etc/modprobe.d/win3-goodix.conf
  ```
  install goodix /sbin/modprobe --ignore-install goodix ; /sbin/modprobe -r --ignore-install goodix ; /sbin/modprobe --ignore-install goodix
  ```
  sudo nano /etc/X11/xorg.conf.d/99-touchscreen.conf
  ```
  Section "InputClass"
  Identifier    "calibration"
  MatchProduct  "Goodix Capacitive TouchScreen"
  Option        "TransformationMatrix"   "0 1 0 -1 0 1 0 0 1"
  EndSection
  ```
## Displaydrehung
/etc/X11/xorg.conf.d/30-monitor.conf
```
Section "Monitor"
Identifier "DSI-1"
Option     "Rotate" "right"
EndSection
```
## Sound
/etc/modprobe.d/alsa-base.conf
```
options snd-intel-dspcfg dsp_driver=1
```
- ## Standby
  ```
  sudo su
  echo s2idle > /sys/power/mem_sleep
  
  sudo grubby --update-kernel=ALL --args="fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up mem_sleep_default=s2idle"
  ```