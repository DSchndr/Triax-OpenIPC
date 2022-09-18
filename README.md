# Triax TTF 2IP / TBF 2 OpenIPC install

## How to do this?

Set your PC IP to `192.168.1.2`, Subnet `/24`

Get a tftp server like Open TFTP Server and start it, drop the files from the archive in the server directory.

In the Webinterface (admin:123456) go to Setup->Network->Port, enable telnet there and save. 

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
