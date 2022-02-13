# [GPD Pocket 3](https://gpd.hk/gpdpocket3)
## Grub-Menü drehen und Standby
nano /etc/default/grub
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet fbcon=rotate:1 mem_sleep_default=s2idle"
GRUB_CMDLINE_LINUX="fbcon=rotate:1"
GRUB_GFXMODE=1200x1920x32
```
update-grub
## Sound
sudo nano /etc/modprobe.d/alsa.conf
```
options snd-intel-dspcfg dsp_driver=1
```
## Capture HDMI input
```
ffplay /dev/video3
```
### Icon für KVM Videocapture
create a new file in ~/.local/share/applications/kvm.desktop with the following
```
[Desktop Entry]
Name=KVM
Comment=Show KVM module in VLC
# change to full path to vlc, i installed it as a flatpak
Exec=flatpak run org.videolan.VLC v4l2:///dev/video2:v4l2-standard= :live-caching=0
Terminal=false
Type=Application
Categories=Utility;
```
Log out and in again or run (for gnome /gtk based desktops)

gtk-update-icon-cache
- # Pipewire
  
  Add ppa for latest build
  ```
  sudo add-apt-repository ppa:pipewire-debian/pipewire-upstream
  ```
  Update
  ```
  sudo apt update
  ```
  Install components
  ```
  sudo apt install gstreamer1.0-pipewire pipewire-media-session libspa-0.2-bluetooth libspa-0.2-jack pipewire pipewire-audio-client-libraries
  ```
  If you get unmet dependencies, you can run:
  ```
  sudo apt --fix-broken install
  ```
  Then re-run
  ```
  sudo apt install gstreamer1.0-pipewire pipewire-media-session libspa-0.2-bluetooth libspa-0.2-jack pipewire pipewire-audio-client-libraries
  ```
  Reload new services
  ```
  systemctl --user daemon-reload
  ```
  Disable PulseAudio service
  ```
  systemctl --user --now disable pulseaudio.service pulseaudio.socket
  ```
  If you update from previous version of PopOS
  ```
  systemctl --user mask pulseaudio
  ```
  Enable Pipewire services
  ```
  systemctl --user --now enable pipewire pipewire-pulse
  ```
  Enable Pipewire media session
  ```
  systemctl --user --now enable pipewire-media-session.service
  ```
### Sensor 

sudo nano /etc/udev/hwdb.d/61-Pocket3-sensor-local.hwdb
für PoP!_OS
```
sensor:modalias:*
ACCEL_MOUNT_MATRIX=0, -1, 0; -1, 0, 0; 1, 0, 0
```
für Fedora
```
sensor:modalias:*
ACCEL_MOUNT_MATRIX=-1, 0, 0; 0, 1, 0; 1, 0, 0
```
sudo systemd-hwdb update
sudo udevadm trigger
### Sound 
sudo nano /etc/modprobe.d/alsa-Pocket3.conf
```
options snd-intel-dspcfg dsp_driver=1
```
### Xorg configs
sudo nano /usr/share/X11/xorg.conf.d/00-CONFIG.config

20-gpd-pocket3-intel.conf
```
Section "Device"
Identifier "Device0"
Driver     "intel"
Option     "TearFree"    "true"
Option     "DRI"         "3"
EndSection
```

40-gpd-pocket3-monitor.conf
```
# GPD Pocket 3 (modesetting)
Section "Monitor"
Identifier "DSI-1"
Option     "Rotate"  "right"
EndSection

# GPD Pocket 3 (xorg-video-intel)
Section "Monitor"
Identifier "DSI1"
Option     "Rotate"  "right"
EndSection
```

80-gpd-pocket3-trackpoint.conf
```
Section "InputClass"
Identifier     "GPD Pocket 3 touchpad"
MatchProduct   "HAILUCK CO.,LTD USB KEYBOARD Mouse"
MatchIsPointer "on"
Driver         "libinput"
Option         "AccelSpeed"      "1"
Option         "MiddleEmulation" "1"
Option         "ScrollButton"    "2"
Option         "ScrollMethod"    "button"
EndSection
```

sudo nano /etc/udev/rules.d/99-Pocket3-touch.rules
```
ACTION=="add", KERNEL=="event[0-9]*", ATTRS{name}=="GXTP7380:00 27C6:0113", ENV{LIBINPUT_CALIBRATION_MATRIX}="1 0 0 0 1 0 0 0 1"
ACTION=="add", KERNEL=="event[0-9]*", ATTRS{name}=="GXTP7380:00 27C6:0113 Stylus Pen (0)", ENV{LIBINPUT_CALIBRATION_MATRIX}="0 1 0 -1 0 1"
ACTION=="add", KERNEL=="event[0-9]*", ATTRS{name}=="GXTP7380:00 27C6:0113 Stylus Eraser (0)", ENV{LIBINPUT_CALIBRATION_MATRIX}="0 1 0 -1 0 1"
```

sudo nano /usr/share/glib-2.0/schemas/90-Pocket3.gschema.override
```
[org.ArcticaProject.arctica-greeter]
enable-hidpi='on'
```
- ### Kernel config
  ```
  GRUB_CMDLINE_LINUX="${GRUB_CMDLINE_LINUX} fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up mem_sleep_default=s2idle"
  GRUB_GFXMODE=1200x1920x32
  ```
  sudo grubby --update-kernel=ALL --args="fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up mem_sleep_default=s2idle"
- ## Bios Hack
  ``ctrl`` + ``h`` nach dem einschalten Drücken, dann ``DEL`` drücken nun ist das Bios freigeschaltet und alle Optionon sind verfügbar.
  
  ``When the BIOS copyright and prompt appears on your screen
  press ctrl-h, your screen should blank out for a moment
  press DEL to enter the BIOS``