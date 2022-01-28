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



# GPD WIN 3

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
# GPD Pocket 3

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

[Desktop Entry]
Name=KVM
Comment=Show KVM module in VLC
# change to full path to vlc, i installed it as a flatpak
Exec=flatpak run org.videolan.VLC v4l2:///dev/video2:v4l2-standard= :live-caching=0
Terminal=false
Type=Application
Categories=Utility;

Log out and in again or run (for gnome /gtk based desktops)

gtk-update-icon-cache

## 
