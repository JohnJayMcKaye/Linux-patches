# Linux-patches
Patches for devices: GPD WIN 3, GPD Pocket 3, Lenovo X1 YOGA

# Allgemein
## Gnome Fractional Scaling

on Wayland
```
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
```
on X11
```
gsettings set org.gnome.mutter experimental-features "['x11-randr-fractional-scaling']"
```
## Icons
sudo apt install papirus-icon-theme








# Enable VA-API 

## Firefox
about:config 

media.ffmpeg.vaapi.enabled set to 			true
media.ffvpx.enabled set to 				false
media.rdd-vpx.enabled set to 				false
media.navigator.mediadatadecoder_vpx_enabled set to 	true


install intel-gpu-tools
sudo intel_gpu_top
zum überprüfen

install vainfo
vainfo
zeigt die Fähigkeiten der Grafikkarte an

## OS 

install mesa-va-drivers für NVIDIA und AMD

install intel-media-va-driver für INTEL

## Firefox Touch scrolling
sudo nano /etc/security/pam_env.conf
```
MOZ_USE_XINPUT2 DEFAULT=1
```
set about:config to dom.w3c_touch_events.enabled=1 (default 2)






## [Surface Maus](https://www.microsoft.com/de-de/d/surface-maus/8qbtdr3q4rpw?activetab=pivot%3aoverviewtab)

    Comment out the only non-commented line in file /lib/udev/rules.d/50-bluetooth-hci-auto-poweron.rules
    
    Uncomment lines [Policy] and AutoEnable=true (originaly there is =false, change it) in /etc/bluetooth/main.conf
    
Reboot
    
Search and pair your mouse. If cursor is not moving, try pairing again.





## Virtualbox 
```
sudo gpasswd -a $USER vboxusers
```






# [GPD WIN 3](https://www.gpd.hk/gpdwin3)

## touchscreen
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
## Standby
```
sudo su
echo s2idle > /sys/power/mem_sleep
```




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

## Pipewire

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











# [Kcosit K72H](https://www.kcosit.com/Rugged-Handheld-PDA-Win10-1D2D-Barcode-Scanner-4GB-RAM-64GB-ROM-p1563970.html)

sudo mkdir /lib/firmware/silead

sudo cp firmware_00.fw /lib/firmware/silead/mssl1680.fw

sudo apt-get install xserver-xorg-input-evdev xserver-xorg-core

sudo nano /usr/share/X11/xorg.conf.d/10-evdev.conf 
```
Section "InputClass"
        Identifier "evdev touchscreen catchall"
        MatchIsTouchscreen "on"
        MatchDevicePath "/dev/input/event*"
        Driver "evdev"
        Option "SwapXY" "0"
EndSection
```

sudo nano /usr/share/X11/xorg.conf.d/99-calibration.conf
```
Section "InputClass"
      Identifier "evdev touchscreen catchall"
      MatchIsTouchscreen "on"
      MatchDevicePath "/dev/input/event*"
      Driver "evdev"
      Option "InvertX" "1"
      Option "InvertY" "1"
      Option "Calibration" "15 875 15 1650"
      Option "SwapAxes" "1"
EndSection
```
Rechtscklick:
Option "EmulateThirdButton" "1"
Option "EmulateThirdButtonTimeout" "750"
Option "EmulateThirdButtonThreshold" "30"

wenn das UI zu groß ist:

$ sudo xrandr --output DSI1 --scale 2x2





