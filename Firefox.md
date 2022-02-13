- ### [[Firefox]] VAAPI
  id:: 6206727e-1ad9-48d3-9daa-92ce7016e86a
  Videobeschleunigung aktivieren VAAPI=VA-API
  
  install mesa-va-drivers für NVIDIA und AMD und ffmpeg libva libva-utils.
  install libva-intel-driver (bzw. intel-media-va-driver) für INTEL und ffmpeg libva libva-utils.
  FEDORA bringt schon alles mit es muss nichts weiter installiert werden
  
  `about:config`
  ```
  gfx.webrender.enabled									true
  widget.wayland-dmabuf-vaapi.enabled						true
  media.ffmpeg.vaapi.enabled set to 						true
  media.ffvpx.enabled set to 								false
  media.rdd-vpx.enabled set to 							false
  media.navigator.mediadatadecoder_vpx_enabled set to 	true
  ```
  
  ```
  
  install intel-gpu-tools
  sudo intel_gpu_top
  zum überprüfen
  ```
  ```
  install vainfo
  vainfo
  ```
  
  zeigt die Fähigkeiten der Grafikkarte an
- ## Firefox Touch scrolling
  sudo nano /etc/security/pam_env.conf
  ```
  MOZ_USE_XINPUT2 DEFAULT=1
  ```
  set about:config to dom.w3c_touch_events.enabled=1 (default 2)
-