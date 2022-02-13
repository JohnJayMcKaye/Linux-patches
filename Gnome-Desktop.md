- ## #Gnome-Plugins
	- ## [Gnome Plugins installieren](https://extensions.gnome.org/#)
	  ![](https://extensions.gnome.org/extension-data/icons/icon_307_2.png) [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
	  ![](https://extensions.gnome.org/extension-data/icons/icon_1070_KXz9hHX.png) [Syncthing Indicator](https://extensions.gnome.org/extension/1070/syncthing-indicator/)
	  ![](https://extensions.gnome.org/extension-data/icons/icon_750.png) [OpenWeather](https://extensions.gnome.org/extension/750/openweather/)
	  sudo dnf install gnome-shell-extension-openweather
	  
	  ![]([https://extensions.gnome.org/extension-data/icons/icon_1082.png](https://extensions.gnome.org/extension-data/icons/icon_1082.png)) [cpufreq]([https://extensions.gnome.org/extension/945/cpu-power-manager/](https://extensions.gnome.org//extension/1082/cpufreq/))
	  ![](https://extensions.gnome.org/extension-data/icons/icon_3737.png) [Hue Lights](https://extensions.gnome.org/extension/3737/hue-lights/)
	  ![]() [Sound Input & Output Device Chooser](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)
	  ![](https://extensions.gnome.org/extension-data/icons/icon_517.png) [Caffeine](https://extensions.gnome.org/extension/517/caffeine/)
	  ![](https://extensions.gnome.org/extension-data/icons/icon_2890.png) [Tray Icons: Reloaded](https://extensions.gnome.org/extension/2890/tray-icons-reloaded/)
-
-
-
- ### Fraktional scaling
  on Wayland
  ```
  gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
  ```
  on X11
  ```
  gsettings set org.gnome.mutter experimental-features "['x11-randr-fractional-scaling']"
  ```
- ### Gnome Terminal Theme
  ```
  wget https://raw.githubusercontent.com/denysdovhan/gnome-terminal-one/master/one-dark.sh && . one-dark.sh
  ```