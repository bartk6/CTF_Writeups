Network Disk Forensics
======================

Dear bart,

Nobody likes having to download large disk images for CTF challenges so this time we're giving you a disk over the network!

Regards,  
jscarsbrook

AU: `nc chal.2025.ductf.net 30016`  
US: `nc chal.2025-us.ductf.net 30016`  

----
## Solve
- `nc chal.2025.ductf.net 30016` returned `NBDMAGICIHAVEOPT`
- Loaded the NBD kernel module: `sudo modprobe nbd`
- Connected with: `sudo nbd-client -N root chal.2025.ductf.net 30016 /dev/nbd0`
- Mounted the filesystem: `sudo mkdir /mnt/nbd && sudo mount /dev/nbd0 /mnt/nbd`
- Navigated and copied the flag image locally: `cp /mnt/nbd/path/to/flag.jpg ~/flag.jpg`
- Opened the image to read the flag: `DUCTF{now_you_know_how_to_use_nbd_4y742rr2}`
