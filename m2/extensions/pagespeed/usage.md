---
layout: default
title: Pagespeed Usage
description: How to setup email package
keywords: "pagespeed setup usage guide"
category: Pagespeed
---

# Pagespeed setup


### Lets start

Check your site pagespeed rank and take results screenshot
We recomend use [Google Page speed insights online tool](https://developers.google.com/speed/pagespeed/insights)
Usally we also check results at [Gtmetrix online tool](http://gtmetrix.com).

Open configuration

`Store > Configuration` > `Swissup > Pagespeed`

In Main section

Set Enable Yes

~~~
php bin/magento config:show pagespeed/main/enable
php bin/magento config:set pagespeed/main/enable 1
~~~


If your store in developer mode. You need to enable developer mode compatability.

Enable in developer mode - Yes

~~~
php bin/magento deploy:mode:show
~~~

Test GZIP compression

When optimizing a website’s performance, the very first thing you should check is whether or not your site is taking advantage of GZIP Compression. If you see green message. All right.
But if you see red error message than you need to enable and test gzip compression support on your web server.

> The HTML5 Boilerplate project contains [sample configuration files](https://github.com/h5bp/server-configs) for all the most popular servers with detailed comments
> for each configuration flag and setting: find your favorite server in the list, look for the gzip section, and confirm that
> your server is configured with recommended settings.


### Apache configuration gzip option example

Check and Enable AllowOverride option in you Apache configuration

~~~
AllowOverride All
~~~

Apache configuration file is located in one of these locations:

/etc/apache2/httpd.conf
/etc/apache2/apache2.conf
/etc/httpd/httpd.conf
/etc/httpd/conf/httpd.conf


Check also
~~~
KeepAlive On
~~~

If you do not have any error you have AllowOverride None or All. Then alter the Deny/Allow to:

~~~
Deny From All
~~~


If you have the 403 result it's a AllowOverride All.

~~~
#Deny From All #Check for AllowOverride All
~~~

Then add the following to your htaccess file and comment 'Deny From All'

~~~
<IfModule mod_php5.c>
    ## enable resulting html compression
   php_flag zlib.output_compression on
</IfModule>

<IfModule mod_deflate.c>

    SetOutputFilter DEFLATE
    AddOutputFilterByType DEFLATE text/html text/css text/plain text/xml application/x-javascript application/x-httpd-php
    BrowserMatch ^Mozilla/4 gzip-only-text/html
    BrowserMatch ^Mozilla/4\.0[678] no-gzip
    BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
    BrowserMatch \bMSI[E] !no-gzip !gzip-only-text/html
    SetEnvIfNoCase Request_URI \.(?:gif|jpe?g|png)$ no-gzip

</IfModule>

Header set Connection keep-alive

# Expires Headers - 2678400s = 31 days
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresDefault "access plus 1 seconds"
  ExpiresByType text/html "access plus 7200 seconds"
  ExpiresByType image/gif "access plus 2678400 seconds"
  ExpiresByType image/jpeg "access plus 2678400 seconds"
  ExpiresByType image/png "access plus 2678400 seconds"
  ExpiresByType text/css "access plus 518400 seconds"
  ExpiresByType text/javascript "access plus 2678400 seconds"
  ExpiresByType application/x-javascript "access plus 2678400 seconds"
</IfModule>

# Cache Headers
<IfModule mod_headers.c>
  # Cache specified files for 31 days
  <FilesMatch "\.(ico|flv|jpg|jpeg|png|gif|css|swf)$">
  Header set Cache-Control "max-age=2678400, public"
  </FilesMatch>
  # Cache HTML files for a couple hours
  <FilesMatch "\.(html|htm)$">
  Header set Cache-Control "max-age=7200, private, must-revalidate"
  </FilesMatch>
  # Cache PDFs for a day
  <FilesMatch "\.(pdf)$">
  Header set Cache-Control "max-age=86400, public"
  </FilesMatch>
  # Cache Javascripts for 31 days
  <FilesMatch "\.(js)$">
  Header set Cache-Control "max-age=2678400, private"
  </FilesMatch>
</IfModule>

#Deny From All #Check for AllowOverride All
~~~


##### See also

Great! Now you might want to see previous:

- [Installation](/m2/extensions/pagespeed/installation/)
- [Configuration](/m2/extensions/pagespeed/configuration/)
- [Changelog](/m2/extensions/email/changelog/)