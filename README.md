# How to migrate your WordPress

For this tutorial I'll use [WP Migrate DP](https://wordpress.org/plugins/wp-migrate-db/) plugin since so far this is the easiest way to move a WP install I've found.

**Note**: While developing your theme, try to use relative paths for your images. WP will set the path for your image to an absolute path e.g. `http://dev.yourproject.com/wp-content/uploads/2016/03/your-image.png`, shrink that to only `/wp-content/uploads/2016/03/your-image.png` and you'll avoid future reworking since neither WP Migrate DB or your editions to the functions file will change the path of the images.

The process is quite straightforward:

1. After installing the plugin head over to its configuration at `Tools > Migrate DB` and fill the address and path of your current project and the equivalent information for the destination:
```
Find                                       Replace
//dev.yourproject.com                      yourproject.com
C:/xampp/htdocs/dev-project-folder         /var/www/prod-project-folder
```
Check your *Advanced Options*. I checked *Replace GUIDs*, *Exclude spam comments*, *Exclude transients (temporary cached data)* and *Exclude post revisions*. Hit **Migrate DB** and download the dump file which I named `dump.sql`.

**Note**: I have set my development database with the same credentials of my production database. Same database name, user and privileges. ***Check your user privileges in production***.

2. Edit your `wp-config.php` to [change the project's URL](https://codex.wordpress.org/Changing_The_Site_URL). At the top of your file after `<?php` open tag include:
```
define('WP_HOME','http://yourproject.com');
define('WP_SITEURL','http://yourproject.com');
```
Now your development environment will stop working but we will fix this in a second.

3. Zip the whole `dev-project-folder` and upload it to your production server.

4. Import your database (the dump file generated at Step 1) with `$ mysql -u username -p name_of_your_production_db < dump.sql` or your preferred alternative e.g. MySQL Workbench or HeidiSQL. 

Everything **should** be working at your production environment by now. Check for 404s because you'll likely have some, mainly images you forgot to set a relative path.

5. Back to your development site open you wp-config file again and set `WP_HOME` and `WP_SITEURL` to its former location e.g. `http://dev.yourproject.com`. Refresh dev.yourproject.com a few times and everything should be functioning again. That's all folks!
