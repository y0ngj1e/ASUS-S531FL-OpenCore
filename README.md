# ASUS Vivobook S15 S531FL Hackintosh

![Alt text](https://github.com/y0ngj1e/ASUS-S531FL-OpenCore/raw/main/macOS%20Sonoma.png)

**OpenCore version:** 1.0.0

**Guide used:** https://dortania.github.io/OpenCore-Install-Guide/

[Geekbench 6 Score](https://browser.geekbench.com/v6/cpu/6947236)

Updated 17 July 2024 for macOS 14.5, will update again when 15.0 is released

This EFI may work on other ASUS laptops with similar specs, use it at your own risk!

You may use older versions of macOS with this EFI, just replace AirportItlwm.kext with your desired version accordingly and rebuild CPUFriendDataProvider.kext

# My Specs
- [ASUS Official Website](https://www.asus.com/laptops/for-home/vivobook/vivobook-s15-s531/)
- **CPU:** Intel Core i7-8565U
- **GPU:** Intel UHD 620 & Nvidia MX250
- **RAM:** 8GB Internal + 8GB Slotted DDR4 2133MHz
- **Storage:** Intel 660p 512GB
- **Wifi/BT Card:** Intel(R) Dual Band Wireless AC 8265
- **Touchpad:** ELAN1200
- **BIOS revision:** Version 301

# Working:
- [x] CPU & Full Power Management (800MHz min freq)
- [x] GPU QE/CI with 2GB VRAM & Brightness adjustment
- [x] HiDPI with framebuffer patch to run any HiDPI resolution
- [x] Wifi & Bluetooth
- [x] Sleep (S3 state)
- [x] Sound (Internal speaker) with boot chime
- [x] All USB ports including Micro SD slot
- [x] Native battery charging limiter to 83%, toggle in settings

# Not Working:
- UVC IR Webcam
- Nvidia MX250 
- Keyboard Backlight

# Issues:
1) CPU thermal throttles at 70C under heavy load instead of 95C. Clock speed and Power draw drops to 2.5GHz and 12W respectively. This issue also occurs on some real MacBooks. There is currently no known fix.

# Untested:
- HDMI (probably need to patch framebuffer)
- Multicombo headphone jack (probably need VerbStub.kext)
- Apple features such as Continuity, AirDrop, iMessage and FaceTime as I don't have an iOS device. (App Store works)

# Pre Installation:
The EFI folder in this repo is the final setup of this hack. Please follow the official guide and remove unnecessary kexts, SSDTs and modify the config.plist before booting into the installer to install macOS.

- [Get your PlatformInfo serial number here](https://github.com/corpnewt/GenSMBIOS)
- [Refer to this readme for the purpose of each ACPI patch and kexts](https://github.com/y0ngj1e/ASUS-S531FL-Hackintosh-OpenCore/blob/main/ACPI%20%26%20Kexts%20Info.md)

# Post Installation Stuff:

- [Monitor your CPU, GPU, fan speed, memory usage and more](https://github.com/CloverHackyColor/HWMonitorSMC2)

- Enable HiDPI by following steps from [this website](https://codeclou.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/) and with [this app](https://github.com/avibrazil/RDM)

- [Calibrate your display white point with this accessibility option](https://apple.stackexchange.com/questions/426761/reduce-brightness-of-white-color-whitepoint-on-macbook-2020-eye-strain-pain)

- [Build your own Voltageshift.kext for undervolting without disabling SIP](https://github.com/zspherez/VoltageShift/tree/master)

- Automate applying undervolt on boot & waking from sleep with [Hammerspoon](http://www.hammerspoon.org/)

   + Paste this in init.lua (modify to your username)
   + Following code below has power limits set when plugged in/on battery. Change accordingly to suit your needs
``` xml
function sleepWatchFunction(eventType)
  if (eventType == hs.caffeinate.watcher.systemDidWake) then
    batteryWatchFunction()
   end
end

function batteryWatchFunction(eventType)
  hs.execute("/users/yongj1e/voltageshift offset -121 -100 -100 -100 -100 0")
  if (hs.battery.powerSource() == 'AC Power') then
    hs.execute("/users/yongj1e/voltageshift power 25 25")
  else
    hs.execute("/users/yongj1e/voltageshift power 5 5")
  end
end

sleepWatcher = hs.caffeinate.watcher.new(sleepWatchFunction)
sleepWatcher:start()

batteryWatcher = hs.battery.watcher.new(batteryWatchFunction)
batteryWatcher:start()
batteryWatchFunction()
```

- Get a powerful systemwide EQ with Apple's [AU Lab](https://www.apple.com/apple-music/apple-digital-masters/) and [BlackHole](https://github.com/ExistentialAudio/BlackHole)

   + 1) Install both AU Lab & BlackHole.
   + 2) [Follow steps here](https://github.com/ExistentialAudio/BlackHole/issues/4#issuecomment-1166148773)
   + 3) Open AU Lab, select "BlackHole" as input device and your desired audio device as output. 


