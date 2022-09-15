+++
author = "c4ndy"
title = "<wiki> | Reinstalling nextcloud "
date = "2022-08-29"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    
    
]
categories = [
    "self hosting",
    "servers",
    "homelab"
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]

+++

Re-intstalling next cloud, for whatever reason :)

To re-install you should:

1. Stop the web server
	`systemctl stop apache2`

2. delete or rename the `config.php` file sitting in the `<directory>/nextcloud/config directory`. 
I prefer renaming it, just incase
	`cd /var/www/cloud.me.xyz/config`
	`sudo mv config.php config.php.bak `

3. delete or rename your data directory, that is `<directory>/nextcloud/data`
In my case: 
`cd cd /var/www/cloud.me.xyz `
`sudo mv data data.bak`

4. Login to maria db and either drop all the tables in your database to generate a list of SQL statements you need to drop all the tables. Cut and paste back into the SQL cli. Use something like:

>`sudo mysql`
> `use nextcloud;`
> `SELECT concat("DROP TABLE IF EXISTS ", table_name, ";")
 FROM information_schema.tables WHERE table_schema = "nextcloud";`
 
 or just drop the database entirely, and create a new one, give it the permissions
 
 >`DROP DATABASE nextcloud;`
 >`CREATE DATABASE nextcloud;`
> `SHOW DATABASES;`
> `GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY '<password>';`


5. Finally, go back to the `<directory>/nextcloud/config` directory and create a new file CAN_INSTALL using something like this:

`cd /var/www/nextcloud/config`
`touch CAN_INSTALL`
`chown www-data CAN_INSTALL`
`chgrp www-data CAN_INSTALL`
`chmod 644 CAN_INSTALL`

6. Restart the web server



__Conclusion: bitch just use snapd honestly, you can either have a life or be a sys-admin__
