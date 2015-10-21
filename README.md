# Preventing process killing or aborting worpress running LiteSpeed

LiteSpeed web server has been known to kill or stop processes that take more than a few microseconds to run. It does not stop these processes gracefully but simply kills them silently. 
When using software like Wordfence or backup software that needs a little more time to complete certain tasks, this can lead to problems. 
If you are using Wordfence auto-update feature, this may lead to your site becoming unusable if LiteSpeed kills an upgrade halfway through copying files.
To prevent his you need to make a very simple change. 

1. Find your site's .htaccess file. This file usually lives in your website root folder. So it may be in a folder like public_html/.htaccess
2. Open the file with a text editor.
3. Find the area that looks like the following:

        RewriteEngine On
        RewriteBase /
        RewriteRule ^index\.php$ - [L]
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule . /index.php [L]

Add a line after RewriteBase that says: `RewriteRule .* - [E=noabort:1]`
So the new code will look like this:

        RewriteEngine On
        RewriteBase /
        RewriteRule .* - [E=noabort:1]
        RewriteRule ^index\.php$ - [L]
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule . /index.php [L]

 

This will tell LiteSpeed to not abruptly abort requests. 
That should allow your site to update correctly, allow Wordfence scans to run to completion and also allow any backup plugins on your WordPress site to function without problems. 
