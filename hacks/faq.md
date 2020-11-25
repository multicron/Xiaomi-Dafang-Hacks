### Overview
The cameras supported by Xiaomi-Dafang-Hacks (even brand-new stock cameras) run a tiny version of Linux as their firmware.  This firmware is stored in non-volatile RAM (NVRAM) in the camera's hardware.  When you update the firmware of the camera, it is this tiny version of Linux that is updated.  There is also a boot loader installed in the NVRAM, similar to the BIOS of a PC, which handles reading the Linux system out of the NVRAM into RAM and starting it running.  The boot loader does not need to be modified to use Xiaomi-Dafang-Hacks, but it can be modified for more advanced installations.

When you download and install the Xiaomi-Dafang-Hacks firmware file for your camera, you are installing a very slightly modified version of the stock firmware for your camera. This is called "The Custom Firmware".  A single file (named rcfile.sh) has been changed in the stock firmware's tiny Linux system to tell it to check for the presence of a different, separate tiny Linux system on the microSD card installed in the camera.

This separate system, installed on the microSD card, is called "The CFW Files". These are the files that contain all the new and changed functionality contained in Xiaomi-Dafang-Hacks. If an appropriate installation of the CFW Files is found on the microSD card, the programs on it are used as the operating system of the camera instead of the programs in the slightly-modified firmware's tiny Linux system stored in the NVRAM.  If there is no microSD card installed, or it does not include the CFW Files, the slightly-modified firmware ("The Custom Firmware") continues executing normally, and the camera behaves as it would if no modification to the stock firmware had taken place.

It is worth noting that the Custom Firmware is a slight modification of a particular version of the manufacturer's stock firmware.  If you update the camera to a different version of the stock firmware, you will overwrite the Custom Firmware and the camera will be reverted to a completely stock camera, because the special file that checks for the CFW files on the microSD card will no longer be present in the firmware stored in NVRAM.

These two elements, "The Custom Firmware" and "The CFW Files", are together referred to as "The CFW".  Despite the names, only the Custom Firmware is an actual firmware installed into NVRAM on the camera, and although it is "Custom", only one file has been slightly customized.  The CFW Files are a separate Linux filesystem stored on the microSD card that is mounted on top of the firmware's file system, causing the camera to operate with programs stored on the microSD card instead of the files stored in the firmware.

### Does the CFW completely replace the stock firmware? 
No.  Because the CFW gets control of the system after it has partially booted, the CFW cannot replace the running Linux kernel that is started by the slightly modified stock firmware, unless you [flash a custom bootloader](/hacks/flashinguboot.md), which is a more dangerous process than just installing the Custom Firmware.

### Does the CFW contain a RTSP-Server? 
Yes, you can watch it through VLC Player.

### Does the CFW connect to any Xiaomi Servers?
No. It does not connect to anything.

### Does the CFW remove the stock firmware?
No. You can still boot the slightly modified stock firmware if you remove the Micro SD card.

### Is it possible to run the stock firmware and the CFW at the same time?
No, it's not possible and it's very unlikely that this will change in the future.

### Can I revert the CFW back to stock firmware?
Yes, you can. However there is no need to revert it back. If your Micro SD card does not contain the CFW Files, your camera will just boot the slightly modified stock software. If you still want to revert back to a stock firmware just flash the appropriate one for your camera from the firmware_original folder, or any other firmware file from the manufacturer, the same way you flashed the custom firmware.

### Is it possible to run the CFW without a Micro SD card?
While it can be done, there will be a lot of trouble in getting it to work and is outside the scope of this project.  One of the big advantages of the CFW is that it can be easily modified just by changing the files on the Micro SD card.

### Can I have FullHD resolution?
Yes, but you must [flash a custom bootloader](/hacks/flashinguboot.md) to achieve this.

### Can the camera send a multicast stream?
Yes, uncomment and customize the multicast destination IP and port in /system/sdcard/config/rtspserver.conf and restart.

### Can I use USB ethernet cards?
Yes, just create a usb_eth_driver.conf file in /system/sdcard/config.
```
cp /system/sdcard/config/usb_eth_driver.conf.dist /system/sdcard/config/usb_eth_driver.conf
reboot
```
If this file exists the run.sh won't start the WIFI driver but will instead load the usb ethernet driver. Currently only the asix.ko driver is supported but others can be built.

### Which Features does the CFW contain?
- Full working RTSP with H264/MJPEG. Based on https://github.com/mpromonet/v4l2rtspserver
- SSH-Server(dropbear) with username: root password: ismart12
- FTP-Server(bftpd) with username: root password: ismart12
- Webserver(lighttpd) with username: root password: ismart12
- Image-Snap(Get Jpeg Image) 
- Horizontal/vertical motor rotation / move to center
- Turn on/off blue/yellow/IR LEDs/IR-Cut
- Local h264 recording possible:
```
/system/sdcard/bin/h264Snap > /system/sdcard/video.h264
```
- Audio recording/playing possible:
```
Playing Audio:
/system/sdcard/bin/audioplay /usr/share/notify/CN/init_ok.wav volume

Recording Audio:
/system/sdcard/bin/ossrecord /system/sdcard/test.wav 
```
- Curl
- MQTT
- Telegram

- Any additional software that you may want can be compiled separately, and there is a toolchain available but you will need to do this yourself.

### What if my scripts in config/userscripts/motiondetection are not executed or mqtt/telegram messages/emails are not sent on motion?
Your camera probably runs out of memory when processing the motion event. This is likely in cameras with 64MB e.g. the Xiaofang 1s. Try to [enable some swap memory](https://github.com/EliasKotlyar/Xiaomi-Dafang-Hacks/blob/master/firmware_mod/config/swap.conf.dist#L4) by copying `swap.conf.dist` to `swap.conf` and setting `SWAP=true`.
