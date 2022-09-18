# Triax TTF 2IP / TBF 2 OpenIPC install

## How to do this?

Set your PC IP to `192.168.1.2`, Subnet `/24`

Get a tftp server like Open TFTP Server and start it, drop the files from the archive in the server directory.

In the Webinterface (192.168.1.10 admin:123456) go to Setup->Network->Port, enable telnet there and save. 

(If you are fast enough to login and run `kill -5 hunter` - telnet is enabled at boottime for a short period :) )

Open a command prompt and run `telnet [camera ip]`
>Login is root:ivideo

Run 
 -  `cd /tmp`
 -  `systools tftp 192.168.1.2 upg`
 -  `systools tftp 192.168.1.2 ipctool`
 -  `ipctool upgrade upgrade.cv300 --force`

or 
-	`cd /tmp && systools tftp 192.168.1.2 upg && systools tftp 192.168.1.2 ipctool && ./ipctool upgrade upg --force &`


Your camera will reboot - DO NOT TOUCH ANYTHING (except you get timeout in tftp log -> replug) - "das Magische UBoot" (the boot-loader *badum tss*) will grab the uImage and rootfs over tftp - overwriting the old chinese spaghetti code from dahua / videopark / whatever

(BTW: streamer has an hardcoded IP, no idea what its purpose is :) )

## Configuration
### Change MAC / IP

Login over SSH, IP is either given over DHCP or 192.168.1.3

`fw_setenv ethaddr e2:08:4f:72:b3:f5`

`fw_setenv ipaddr 192.168.1.16`

>majestic.yaml
```
system:
  webAdmin: enabled
  buffer: 1024
  logLevel: TRACE
image:
  mirror: false
  flip: false
  rotate: none
  contrast: auto
  hue: 50
  saturation: 50
  luminance: auto
osd:
  enabled: true
  template: "%a %e %B %Y %H:%M:%S %Z"
  posX: 0
  posY: 0
nightMode:
  enabled: true
  nightAPI: false
  irSensorPin: 65
  irSensorPinInvert: false
  irCutPin1: 66
  irCutPin2: 64
  backlightPin: 53
records:
  enabled: false
  path: /mnt/mmc/%Y/%m/%d/%H.mp4
  maxUsage: 95
video0:
  enabled: true
  codec: h265
  gopMode: normal
  fps: 25
  rcMode: cbr
  bitrate: 4096
  gopSize: 3
video1:
  enabled: false
jpeg:
  enabled: false
  toProgressive: false
mjpeg:
  size: 640x360
  fps: 5
  bitrate: 1024
audio:
  enabled: false
  volume: auto
  srate: 8000
rtsp:
  enabled: true
  port: 554
hls:
  enabled: true
youtube:
  enabled: false
motionDetect:
  enabled: false
  visualize: true
  debug: true
ipeye:
  enabled: false
watchdog:
  enabled: true
  timeout: 20
isp:
  antiFlicker: 50
  memMode: normal
  lowDelay: false
  sensorConfig: /etc/sensors/imx323_i2c_dc_1080p_line.ini
  slowShutter: disabled
  iqProfile: /etc/sensors/iq/imx323.ini
```
