# tribeca
This project consists in synchronizing a video on 20 Raspberry. 

## Configuration
Flash the ROM on your SD Card, we use Raspbian Jessie lite version, omxplayer, omxplayer-sync and their dependencies.

Omxplayer is a video player that can decode h264 video file : https://github.com/popcornmix/omxplayer.

Omxplayer-sync is a tool to synchronize over the network omxplayer instances : https://github.com/turingmachine/omxplayer-sync.

### Name
A Raspberry is named : *DRK-1-XX*, XX being its number. Ex : DRK-1-5
To change a name you have to :
 - Set it in /etc/hostname
 - Set it in /etc/hosts
 - Execute /etc/init.d/hostname.sh
 - Reboot
 
### IP
A Raspberry has a static IP : *192.168.1.X*, X being its number. Ex : 192.168.11
To set it you have to :
  - Add in /etc/dhcpcd.conf :
   "interface eth0
   static ip_address=192.168.1.X/24
   static routers=192.168.1.254 #Default route 
   static domain_name_servers=8.8.8.8"
  - Reboot

## Launch
To launch the system you have to be connected via SSH to all the rapsberry. 
Simply type the command *omxplayer-sync -lu /path/to/your/video.mp4" on all raspberry except one (which will be the master). This will start all the slaves.
On the last raspberry type : *omxplayer-sync -mu /path/to/your/video.mp4* and they will synchronize each other.

## Issues
Over several test we can say that even if all raspberries are the same (they are clone), they don't act the same. A command launched at the same time on all of them will execute with a delay on each and it is not predictable.

So actually, at each loop some raspberries will synchronize and some won't. They start synchronizing around 6sec because of variable on the code, this is the smallest value acceptable. A lower value will result in more sync over time.
