+++
author = "c4ndy"
title = "<wiki> | Using and external disk as a data directory for nextcloud"
date = "2022-08-22"
description = "Web Security"
tags = [
    "http",
    "cookies",
    "web-security",
    
]
categories = [
    "web development",
    "web security",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]

+++

This article is not a well though out writeup- rather a dump of what to do when I fuck up again and need to reinstall my nextcloud.

I provisioned a 60 GB external hardisk to serve as my data folder in nextcloud.

1. Check if the external device is mounted
`df -h`
![wiki_nextcloudMediaDir1](/images/wiki_nextcloudMediaDir1.png)
> It isnt
2.  check for storage decides:
`lsblk`

Now if you find the external disk, it'll be in /dev, special filesystem that holds a reference to every device attached on your disk

I got the 60 gigs HDD that I provisioned as `sdb1`
![wiki_nextcloudMediaDir2](/images/wiki_nextcloudMediaDir2.png)

2. Create a partition on the new disk. You can use gdisk or fdisk. Since I already have the partition I'll move to next step.
3. Reformat the partition as ext4 just to be safe 
`mkfs.ext4 /dev/sdb1`

4. add it to fstab
- Get the UUID of the HDD
`blkid`

- `sudo nano /etc/fstab`
add the new device:
`/dev/disk/by-uuid/c7c8c8b8-e9f5-4c21-8b74-a530db0f50dc /media/nextcloud ext4 defaults 0 2`

This is how my fstab looks like:
![wiki_nextcloudMediaDir3](/images/wiki_nextcloudMediaDir3.png)
save it.

Make a dir `media/nextcloud`
5. mount it
`sudo mount -a`

6. Check to make sure it's mounted"
 `df -h`
![wiki_nextcloudMediaDir4](/images/wiki_nextcloudMediaDir4.png)

> it is! We can now use it 

7. change ownership of this folder to www-data
`sudo chown -R www-data:www-data /media/nextcloud
`

8. While setting up netcloud thorugh the webinterface, just use `/media/nextcloud` as the Data folder

9. To check what port maria db is runnig on:
`sudo netstat -tnlp`

