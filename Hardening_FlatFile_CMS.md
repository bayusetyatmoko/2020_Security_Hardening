## Hardening FlatFile CMS (No Database, No SQL Injection)
A. <br>
#--- Why Using Flat File CMS, <br>
https://www.cmscritic.com/awards/ <br>
https://www.cmscritic.com/flat-file-cms/ <br>
https://www.ionos.com/digitalguide/hosting/cms/flat-file-cms-the-alternative-to-wordpress-etc/ <br>
https://www.cmswire.com/digital-experience/15-flat-file-cms-options-for-lean-website-building/ <br>
https://buttercms.com/blog/introduction-to-flat-file-cms-deciding-whether-they-are-right-for-you/ <br>
<br>
'1. Security, <br>
Since the system is simple, there are fewer chances of errors. However, if you forget the structure <br>
and let an error run through in the architecture, that counts as a major mistake. <br>
A simple folder structure is easily manageable and does not have many dependencies. <br>
This can be said for external security too: SQL is a favorite choice for malicious attacks. <br>
By constantly inserting SQL commands, attackers can mess with the data. <br>
With flat-file CMSs, you don’t have to worry about that. <br>
<br>
'2. Backup, <br>
Flat-file CMSs are easy to back up—simply copy and paste! For more complex systems, <br>
you can routinely back up to effectively save system data, databases, and integrated files. <br>
Flat-file CMSs require a back-up copy, and you can save it, for example, on a USB drive. <br>
<br>
'3. Moving, <br>
WordPress, Typo3, and Drupal all require a shift of servers when moving websites, <br>
and that takes time. Oftentimes, relocating a website that is based on a flat-file CMS <br>
just requires copying over the files. <br>
<br>
'4. Speed, <br>
For smaller website projects, a relational DBMS overshoots the mark <br>
and is not really needed. By simplifying the structure in a flat file system, <br>
you can achieve higher speeds. <br>
<br>
'5. Simplicity, <br>
Large databases usually have a very complex structure, which is held together by lots of links. <br>
As a beginner, you can easily make a mistake, causing the database to collapse like a house of cards. <br>
Since flat file CMSs are based on a simple folder structure, you make less mistakes. <br>
Therefore, these systems are very well suited for people who have little knowledge of databases <br>
and don’t need a large database for their website project. <br>
<br>
'6. Workflow, <br>
If you’re using a classic CMS, editing the content is bound to the backend. <br>
However, if you want to make changes in a flat file CMS or add new content, <br>
you can use your favorite editor. <br>
<br>
B. <br>
#--- Why Using Grav CMS, <br>
https://www.cvedetails.com/product/59205/?q=Grav+Cms <br>
https://learn.getgrav.org/16/security/users <br>
https://learn.getgrav.org/16/security/developers <br>
https://learn.getgrav.org/16/security/server-side <br>
<br>
#--- Refference Grav CMS, <br>
https://getgrav.org/ <br>
https://getgrav.org/features <br>
https://getgrav.org/blog <br>
https://learn.getgrav.org/16 <br>
https://learn.getgrav.org/15 <br>
https://learn.getgrav.org/15/basics/requirements <br>
https://getgrav.org/downloads <br>
https://getgrav.org/downloads/skeletons <br>
https://getgrav.org/downloads/themes <br>
https://getgrav.org/downloads/plugins <br>
https://github.com/getgrav/grav <br>
https://github.com/getgrav/grav/releases <br>
https://github.com/getgrav/grav/releases/tag/1.5.10 <br>
<br>
C. <br>
'01. <br>
#--- HowTo Install Grav CMS, <br>
#--- Download : https://github.com/getgrav/grav/releases/download/1.5.10/grav-admin-v1.5.10.zip <br>
#--- Webroot Location : /var/www/webcms/ <br>
#--- URL Website : http://webcms.idjvnix.com/ <br>
cd ~/ <br>
curl -LJO "https://github.com/getgrav/grav/releases/download/1.5.10/grav-admin-v1.5.10.zip" <br>
unzip grav-admin-v1.5.10.zip <br> 
mv ~/grav-admin/* /var/www/webcms/ <br>
mv ~/grav-admin/.htaccess /var/www/webcms/ <br>
rm -rf ~/grav-admin <br>
<br>
'02. <br>
#--- Setting .htaccess - Grav CMS <br>
nano /var/www/webcms/.htaccess <br>
```
<IfModule mod_rewrite.c>
RewriteEngine On
## Begin RewriteBase
RewriteBase /
## End - RewriteBase
## Begin - X-Forwarded-Proto
# In some hosted or load balanced environments, SSL negotiation happens upstream.
# In order for Grav to recognize the connection as secure, you need to uncomment
# the following lines.
# RewriteCond %{HTTP:X-Forwarded-Proto} https
# RewriteRule .* - [E=HTTPS:on]
## End - X-Forwarded-Proto
## Begin - Exploits
# Block out any script trying to base64_encode data within the URL.
RewriteCond %{QUERY_STRING} base64_encode[^(]*\([^)]*\) [OR]
# Block out any script that includes a <script> tag in URL.
RewriteCond %{QUERY_STRING} (<|%3C)([^s]*s)+cript.*(>|%3E) [NC,OR]
# Block out any script trying to set a PHP GLOBALS variable via URL.
RewriteCond %{QUERY_STRING} GLOBALS(=|\[|\%[0-9A-Z]{0,2}) [OR]
# Block out any script trying to modify a _REQUEST variable via URL.
RewriteCond %{QUERY_STRING} _REQUEST(=|\[|\%[0-9A-Z]{0,2})
# Return 403 Forbidden header and show the content of the root homepage
RewriteRule .* index.php [F]
## End - Exploits
## Begin - Index
# If the requested path and file is not /index.php and the request
# has not already been internally rewritten to the index.php script
RewriteCond %{REQUEST_URI} !^/index\.php
# and the requested path and file doesn't directly match a physical file
RewriteCond %{REQUEST_FILENAME} !-f
# and the requested path and file doesn't directly match a physical folder
RewriteCond %{REQUEST_FILENAME} !-d
# internally rewrite the request to the index.php script
RewriteRule .* index.php [L]
## End - Index
## Begin - Security
# Block all direct access for these folders
RewriteRule ^(\.git|cache|bin|logs|backup|webserver-configs|tests)/(.*) error [F]
# Block access to specific file types for these system folders
RewriteRule ^(system|vendor)/(.*)\.(txt|xml|md|html|yaml|yml|php|pl|py|cgi|twig|sh|bat)$ error [F]
# Block access to specific file types for these user folders
RewriteRule ^(user)/(.*)\.(txt|md|yaml|yml|php|pl|py|cgi|twig|sh|bat)$ error [F]
# Block all direct access to .md files:
RewriteRule \.md$ error [F]
# Block all direct access to files and folders beginning with a dot
RewriteRule (^|/)\.(?!well-known) - [F]
# Block access to specific files in the root folder
RewriteRule ^(LICENSE\.txt|composer\.lock|composer\.json|\.htaccess)$ error [F]
## End - Security
</IfModule>
# Begin - Prevent Browsing and Set Default Resources
Options -Indexes
DirectoryIndex index.php index.html index.htm
# End - Prevent Browsing and Set Default Resources
```
'03. <br>
#--- Setting Ownership & Permission - Grav CMS <br>
chown -R bayu.www-data /var/www/webcms/* <br>
find /var/www/webcms/ -type f -exec chmod 664 {} \; <br>
find /var/www/webcms/bin -type f -exec chmod 775 {} \; <br>
find /var/www/webcms/ -type d -exec chmod 775 {} \; <br>
find /var/www/webcms/ -type d -exec chmod +s {} \; <br>
<br>
'04. <br>
#--- Browsing http://webcms.idjvnix.com/ (to install Grav CMS) <br>
http://webcms.idjvnix.com/    (frontend) <br>
http://webcms.idjvnix.com/admin    (backend) <br>
[done]. <br>
<br>
D. <br>
#--- Why Using GetSimple CMS, <br>
https://www.cvedetails.com/product/21286/?q=Getsimple+Cms <br>
http://get-simple.info/wiki/security <br>
http://get-simple.info/wiki/security:csrf <br>
http://get-simple.info/wiki/config:htaccess <br>
http://get-simple.info/wiki/how_to:change_admin_password_salted <br>
<br>
#--- Refference GetSimple CMS, <br>
http://get-simple.info <br>
http://get-simple.info/download/ <br>
http://get-simple.info/wiki/installation:requirements <br>
http://get-simple.info/wiki/quick_install <br>
http://get-simple.info/extend/ <br>
http://get-simple.info/extend/all_themes.php <br>
http://get-simple.info/extend/all_plugins.php <br>
https://github.com/GetSimpleCMS/GetSimpleCMS <br>
<br>
E. <br>
'01. <br>
#--- HowTo Install GetSimple CMS, <br>
#--- Download : https://github.com/GetSimpleCMS/GetSimpleCMS/archive/v3.3.16.zip <br>
#--- Webroot Location : /var/www/webcms2/ <br>
#--- URL Website : http://webcms2.idjvnix.com/ <br>
cd ~/ <br>
curl -LJO "https://github.com/GetSimpleCMS/GetSimpleCMS/archive/v3.3.16.zip" <br>
unzip GetSimpleCMS-3.3.16.zip <br>
mv GetSimpleCMS-3.3.16/* /var/www/webcms2/ <br>
rm -rf ~/GetSimpleCMS-3.3.16/ <br>
<br>
'02. <br>
#--- Setting Ownership & Permission - GetSimple CMS, <br>
chown -R bayu.www-data /var/www/webcms2/* <br>
find /var/www/webcms2/ -type f -exec chmod 664 {} \; <br>
find /var/www/webcms2/ -type d -exec chmod 775 {} \; <br>
find /var/www/webcms2/ -type d -exec chmod +s {} \; <br>
<br>
'03. <br>
#--- Browsing http://webcms2.idjvnix.com/admin/ (to install GetSimple CMS) <br>
http://webcms2.idjvnix.com/   (frontend) <br>
http://webcms2.idjvnix.com/admin/    (backend) <br>
<br>
'04. <br>
#--- Setting .htaccess - GetSimple CMS, <br>
nano /var/www/webcms2/.htaccess <br>
```
# override charset
AddDefaultCharset UTF-8
# prevent directory listings
Options -Indexes
# Follow symbolink links, This is required for rewrites on some hosts
Options +FollowSymLinks
# Set the default handler.
DirectoryIndex index.php
# blocks direct access to the XML files - they hold all the data!
<Files ~ "\.xml$">
        <IfModule !mod_authz_core.c>
                Deny from all
        </IfModule>
        <IfModule mod_access_compat.c>
                Deny from all
        </IfModule>
        <IfModule mod_authz_core.c>
                <IfModule !mod_access_compat.c>
                        Require all denied
                </IfModule>
        </IfModule>
</Files>
<Files sitemap.xml>
        <IfModule !mod_authz_core.c>
                Allow from all
        </IfModule>
        <IfModule mod_access_compat.c>
                Allow from all
        </IfModule>
        <IfModule mod_authz_core.c>
                <IfModule !mod_access_compat.c>
                        Require all granted
                </IfModule>
        </IfModule>
</Files>
# handle rewrites for fancy urls
<IfModule mod_rewrite.c>
        RewriteEngine on
        # Usually RewriteBase is just '/', but
        # replace it with your subdirectory path
        RewriteBase /
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule /?([A-Za-z0-9_-]+)/?$ index.php?id=$1 [QSA,L]
</IfModule>
```
[done]. <br>
<br>
Surabaya, 08/08/2020, <br>
Hope it can be useful, <br>
Best Regards, <br>
Bayu S. <br>
<br>
