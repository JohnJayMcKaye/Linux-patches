# Linux-patches
Patches for devices: GPD WIN 3, GPD Pocket 3, Lenovo X1 YOGA

# Allgemein
## Gnome Fractional Scaling

## System Update
```
sudo apt update
sudo apt upgrade
sudo apt dist-upgrade
sudo apt autoremove
sudo apt autoclean
sudo fwupdmgr get-devices
sudo fwupdmgr get-updates
sudo fwupdmgr update
flatpak update
```





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



# CODECS
```
sudo apt install -y ubuntu-restricted-extas ffmpeg libavcodec-extra libdvd-pkg; sudo dpkg-reconfigure libdvd-pkg
```





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

### Kernel config
```
GRUB_CMDLINE_LINUX="${GRUB_CMDLINE_LINUX} fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up mem_sleep_default=s2idle"
GRUB_GFXMODE=1200x1920x32
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






# JohnJayMcKaye`s Fedora Setup 

Nachfolgend beschreibe ich mein optimales Fedora Setup.


## Gerätename unter Einstellungen>Info ändern (Standard wert fedora)

## Automatische Anmeldung nach Computerstart einrichten
da die Festplatte ohnehin verschlüsselt ist, so du dein Passwort nicht 2 mal eingeben beim Computerstart und er ist dennoch sicher. 
Einstellungen>Benutzer “Entsperren“ ”Automatische Anmeldung” aktivieren

## [Gnome Plugins installieren](https://extensions.gnome.org/#)
[]!(https://extensions.gnome.org/extension-data/icons/icon_307_2.png) [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
[]!(https://extensions.gnome.org/extension-data/icons/icon_1070_KXz9hHX.png) [Syncthing Indicator](https://extensions.gnome.org/extension/1070/syncthing-indicator/)
[]!(https://extensions.gnome.org/extension-data/icons/icon_750.png) [OpenWeather](https://extensions.gnome.org/extension/750/openweather/)
[]!(https://extensions.gnome.org/extension-data/icons/icon_945_ysaMhAs.png) [CPU Power Manager](https://extensions.gnome.org/extension/945/cpu-power-manager/)
[]!(https://extensions.gnome.org/extension-data/icons/icon_3737.png) [Hue Lights](https://extensions.gnome.org/extension/3737/hue-lights/)
[]!(https://extensions.gnome.org/extension-data/icons/icon_906.png) [Sound Input & Output Device Chooser](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)
[]!(https://extensions.gnome.org/extension-data/icons/icon_517.png) [Caffeine](https://extensions.gnome.org/extension/517/caffeine/)
[]!(https://extensions.gnome.org/extension-data/icons/icon_2890.png) [Tray Icons: Reloaded](https://extensions.gnome.org/extension/2890/tray-icons-reloaded/)
[]!() []()


### TLP
git clone https://github.com/d4nj1/TLPUI.git
cd TLPUI
python3 -m tlpui


## Programme

Flathup-Repo hinzufügen
´´´
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
´´´

[Appimagelauncher](https://github.com/TheAssassin/AppImageLauncher/releases).rpm


Syncthing
´´´
sudo dnf install syncthing
´´´

Hugo
´´´
sudo dnf install hugo
´´´

Papierus-Icon-Theme
´´´
sudo dnf install papirus-icon-theme
´´´

HTTrack Website Copier
´´´
sudo dnf install httrack
´´´

### Software aus dem Software Appstore
0 A.D.
AppImage Pool
Audacity
AusweisApp2
Banking
Blender
Cool Retro Term
Cozy
darktable
Decoder
EasyTAG
Element
Erweiterungs Manager
Geary
Getting Things GNOME!
GNOME Netzwerkbildschirme
GNOME-Optimierung
GNU Image Manipulation Program
Handbrake
gPodder
gThumb Bildbetrachter
Inkscape
JDownloader
Joplin
Kdenlive
KeePassXC
Krita
Logseq
Lutris
Marble
Meteo
nuclear music player
OBS Studio
RawTherapee
ScummVM
Skype
Steam
Telegram Desktop
Tipp10
Vintage Story
VLC
Xournal++
 	-unvollständig/Appimages-
 	
### Appimages
[Jaxx Liberty for Desktop](https://www.jaxx.io/downloads)
[linphone](https://www.linphone.org/)
[QMapShack](https://github.com/Maproom/qmapshack/releases)
[mediathekview](https://mediathekview.de/download/) oder als [.rpm](https://mediathekview.de/download/)
 	









## [VirtualBox 6 auf Fedora Installieren](https://computingforgeeks.com/how-to-install-virtualbox-on-fedora-linux/)

Erstmal Fedora auf aktuellen Stand bringen, also Updaten und einmal neustarten.
´´´
sudo dnf -y upgrade
sudo reboot
´´´

Abhängigkeiten Installieren
´´´
sudo dnf -y install @development-tools
sudo dnf -y install kernel-headers kernel-devel dkms elfutils-libelf-devel qt5-qtx11extras
´´´

VirtualBox Repository zu Fedora hinzufügen 
´´´
cat <<EOF | sudo tee /etc/yum.repos.d/virtualbox.repo 
[virtualbox]
name=Fedora $releasever - $basearch - VirtualBox
baseurl=http://download.virtualbox.org/virtualbox/rpm/fedora/35/\$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.virtualbox.org/download/oracle_vbox.asc
EOF
´´´
Import VirtualBox GPG Key (abfrage ggf. mit j bestätigen)
´´´
sudo dnf search virtualbox
´´´
VirtualBox nun endlich installieren
´´´
sudo dnf install VirtualBox-6.1
´´´
VirtualBox-Benutzer zum System hinzufügen sonst funktionieren einige Dinge nicht, z.B. USB durchreichung.
´´´
sudo usermod -a -G vboxusers $USER
´´´
[VirtualBox 6.1.32 Oracle VM VirtualBox Extension Pack](https://download.virtualbox.org/virtualbox/6.1.32/Oracle_VM_VirtualBox_Extension_Pack-6.1.32.vbox-extpack) herunterladen und installieren. (doppelklicken)













