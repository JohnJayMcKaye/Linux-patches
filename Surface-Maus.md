## [Surface Maus](https://www.microsoft.com/de-de/d/surface-maus/8qbtdr3q4rpw?activetab=pivot%3aoverviewtab)

  Comment out the only non-commented line in file /lib/udev/rules.d/50-bluetooth-hci-auto-poweron.rules
  
  Uncomment lines [Policy] and AutoEnable=true (originaly there is =false, change it) in /etc/bluetooth/main.conf
  
Reboot
  
Search and pair your mouse. If cursor is not moving, try pairing again.