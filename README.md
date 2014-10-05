Arch-Linux-personal-tweaks-collection
=====================================

Personal collection of Arch Linux tweaks for my eeepc 1011px taken everywhere (ArchWiki and web in general ...)


**Mount /tmp /var/log and /var/tmp in RAM and move Chromium/Chrome Cache in tmpfs (Less wear and tear on Hd and tmp faster on ram)**

- nano /etc/fstab
- add this at the end of the file:
```
/var/log  /var/log   tmpfs  defaults,noatime,nodiratime,mode=0755  0 0
/var/tmp  /var/tmp   tmpfs  defaults,noatime,nodiratime,mode=1777  0 0
/tmp      /tmp       tmpfs  defaults,size=600m  0 0
tmpfs   /home/<USER>/.cache     tmpfs   noatime,nodev,nosuid,size=400M  0       0
```

**Tweak swappiness**

- Create 99-sysctl.conf

sudo nano /etc/sysctl.d/99-sysctl.conf

- Add the following content:
```
vm.swappiness=1 
vm.vfs_cache_pressure=50
```

**Disable ipv6 for faster browsing**

- Create a file called 40-ipv6.conf inside /etc/sysctl.d 

sudo nano /etc/sysctl.d/40-ipv6.conf

- Add the following content:
```
# Disable IPv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.interface0.disable_ipv6 = 1
...
net.ipv6.conf.interfaceN.disable_ipv6 = 1
```

**Profile sync daemon (Reduced wear to physical drives)**
```
- yaourt -S profile-sync-daemon

- sudo nano /etc/psd.conf

- Uncomment and add your username to this line
USERS="<USER>"

- Uncomment and add the browser You're using to this line in this way
BROWSERS="chromium opera midori"

- sudo systemctl enable psd.service
```

**prelink**
```
- yaourt -S prelink

- prelink -amR

- Daily cron job:

. Create the folder cron.daily inside inside /etc

sudo mkdir /etc/cron.daily

. Create prelink file 

sudo nano /etc/cron.daily/prelink

with the following content:

#!/bin/bash
[[ -x /usr/bin/prelink ]] && /usr/bin/prelink -amR &>/dev/null

and give ownership permissions

sudo chmod 755 /etc/cron.daily/prelink
```

**preload**
```
- yaourt -S preload

- sudo systemctl enable preload.service
```

**laptop-mode-tools to increase battery life**
```
- yaourt -S laptop-mode-tools 

- sudo systemctl enable laptop-mode.service

- sudo nano /etc/laptop-mode/laptop-mode.conf and enable laptop mode on battery

ENABLE_LAPTOP_MODE_ON_BATTERY=1
```

**powertop to increase battery life**
```
- yaourt -S powertop

- powertop --calibrate

- powertop on boot

. Create a file caller powertop.service

/etc/systemd/system/powertop.service

. add the following content:

[Unit]
Description=Powertop tunings

[Service]
Type=oneshot
RemainAfterExit=no
ExecStart=/usr/bin/powertop --auto-tune
# "powertop --auto-tune" still needs a terminal for some reason. Possibly a bug?
Environment="TERM=xterm"

[Install]
WantedBy=multi-user.target

- Start the service

sudo systemctl enable powertop.service
```


... more to come


