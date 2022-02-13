- #Fedora Linux mit #Gnome-Desktop und #Flatpak
- Nachfolgend beschreibe ich mein optimales Fedora Setup.
	- Gerätename unter Einstellungen>Info ändern (Standard wert fedora)
	- Automatische Anmeldung nach Computerstart einrichten
	  da die Festplatte ohnehin verschlüsselt ist, so du dein Passwort nicht 2 mal eingeben beim Computerstart und er ist dennoch sicher. 
	  Einstellungen>Benutzer “Entsperren“ ”Automatische Anmeldung” aktivieren
- ## Programme
- ## Software aus dem Repo
	- Flathup-Repo hinzufügen
	  ```
	  flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
	  ```
	  
	  [Appimagelauncher](https://github.com/TheAssassin/AppImageLauncher/releases).rpm
	  
	  
	  Syncthing
	  ```
	  sudo dnf install syncthing
	  ```
	  
	  Hugo
	  ```
	  sudo dnf install hugo
	  ```
	  
	  Papierus-Icon-Theme
	  ```
	  sudo dnf install papirus-icon-theme
	  ```
	  
	  HTTrack Website Copier
	  ```
	  sudo dnf install httrack
	  ```
- ### Software aus dem Software Appstore #Flatpak
	- 0 A.D.
	  AppImage Pool
	  Audacity
	  AusweisApp2
	  Banking
	  Blender
	  Calibre
	  Cool Retro Term
	  Cozy
	  darktable
	  Decoder
	  EasyTAG
	  Element
	  Erweiterungs Manager
	  Flatseal
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
	  MPV
	  NoteKit
	  nuclear music player
	  OBS Studio
	  RawTherapee
	  ScummVM
	  Soundconverter (Klangkonverter)
	  Skype
	  Steam
	  Telegram Desktop
	  Tipp10
	  	TextSnatcher
	  VLC
	  Xournal++
		- #VirtualBox
- ### Appimages
	- [Jaxx Liberty for Desktop](https://www.jaxx.io/downloads)
	  [linphone](https://www.linphone.org/)
	  [QMapShack](https://github.com/Maproom/qmapshack/releases)
	  [mediathekview](https://mediathekview.de/download/) oder als [.rpm](https://mediathekview.de/download/)
- ## [VirtualBox 6 auf Fedora installieren](https://computingforgeeks.com/how-to-install-virtualbox-on-fedora-linux/)
  #VirtualBox
  Erstmal Fedora auf aktuellen Stand bringen, also Updaten und einmal neustarten.
  ```
  sudo dnf -y upgrade
  sudo reboot
  ```
  Abhängigkeiten Installieren
  ```
  sudo dnf -y install @development-tools
  sudo dnf -y install kernel-headers kernel-devel dkms elfutils-libelf-devel qt5-qtx11extras
  ```
  
  VirtualBox Repository zu Fedora hinzufügen 
  ```
  cat <<EOF | sudo tee /etc/yum.repos.d/virtualbox.repo 
  [virtualbox]
  name=Fedora $releasever - $basearch - VirtualBox
  baseurl=http://download.virtualbox.org/virtualbox/rpm/fedora/35/\$basearch
  enabled=1
  gpgcheck=1
  repo_gpgcheck=1
  gpgkey=https://www.virtualbox.org/download/oracle_vbox.asc
  EOF
  ```
  Import VirtualBox GPG Key (abfrage ggf. mit j bestätigen)
  ```
  sudo dnf search virtualbox
  ```
  VirtualBox nun endlich installieren
  ```
  sudo dnf install VirtualBox-6.1
  ```
  VirtualBox-Benutzer zum System hinzufügen sonst funktionieren einige Dinge nicht, z.B. USB durchreichung.
  ```
  sudo usermod -a -G vboxusers $USER
  ```
  [VirtualBox 6.1.32 Oracle VM VirtualBox Extension Pack](https://download.virtualbox.org/virtualbox/6.1.32/Oracle_VM_VirtualBox_Extension_Pack-6.1.32.vbox-extpack) herunterladen und installieren. (doppelklicken)
- ## Weitere optimierungen
	- ### TLP
	  :LOGBOOK:
	  CLOCK: [2022-02-10 Thu 12:41:36]--[2022-02-10 Thu 12:41:36] =>  00:00:00
	  CLOCK: [2022-02-10 Thu 12:41:38]
	  :END:
	  ```
	  git clone https://github.com/d4nj1/TLPUI.git
	  cd TLPUI
	  python3 -m tlpui
	  ```