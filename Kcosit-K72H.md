- # [Kcosit K72H](https://www.kcosit.com/Rugged-Handheld-PDA-Win10-1D2D-Barcode-Scanner-4GB-RAM-64GB-ROM-p1563970.html)
  
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