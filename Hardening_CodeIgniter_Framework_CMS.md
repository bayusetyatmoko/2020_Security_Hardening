## Hardening CodeIgniter Framework & CMS
A. <br>
#--- Why Using Framework CodeIgniter, <br>
https://www.cvedetails.com/product/11625/?q=Codeigniter <br>
https://codeigniter.com/userguide3/general/security.html <br>
<br>
'1. Security Feature, <br>
https://codeigniter.com <br>
Strong Security <br>
We take security seriously, with built-in protection against CSRF and XSS attacks. <br>
Version 4 adds context-sensitive escaping and CSP <br>
<br>
'2. Responsive for Security Issues, <br>
https://www.cvedetails.com/product/11625/?q=Codeigniter <br>
https://forum.codeigniter.com/thread-61635.html?highlight=security <br>
(CSRF giving problem with ajax) <br>
<br>
#--- Refference Framework CodeIgniter, <br>
https://codeigniter.com <br>
https://codeigniter.com/download <br>
https://codeigniter.com/userguide3/installation/downloads.html <br>
https://codeigniter.com/userguide3/installation/index.html <br>
https://codeigniter.com/userguide3/overview/getting_started.html <br>
https://codeigniter.com/docs <br>
https://github.com/bcit-ci/CodeIgniter <br>
<br>
B. <br>
'01. <br>
#--- How To Install Framework CodeIgniter <br>
#--- Download : https://api.github.com/repos/bcit-ci/CodeIgniter/zipball/3.1.11 <br>
#--- Webroot Location : /var/www/webci/ <br>
#--- URL Website : http://webci.idjvnix.com/ <br>
cd ~/ <br>
curl -LJO "https://api.github.com/repos/bcit-ci/CodeIgniter/zipball/3.1.11" <br>
unzip bcit-ci-CodeIgniter-3.1.11-0-gb73eb19.zip <br>
mv ~/bcit-ci-CodeIgniter-b73eb19/* /var/www/webci/ <br>
rm -rf ~/bcit-ci-CodeIgniter-b73eb19/ <br>
<br>
'02. <br>
#--- Setting .htaccess - Framework CodeIgniter <br>
nano /var/www/webci/.htaccess <br>
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```
'03. <br>
#--- Setting config.php - Framework CodeIgniter <br>
nano /var/www/webci/application/config/config.php <br>
$config['base_url'] = 'http://webci.idjvnix.com/'; <br>
$config['index_page'] = ''; <br>
<br>
'04. <br>
#--- Setting database.php - Framework CodeIgniter <br>
nano /var/www/webci/application/config/database.php <br>
```
$db['default'] = array(
 'dsn' => '',
 'hostname' => 'localhost',
 'username' => 'webuser',
 'password' => '1234567x',
 'database' => 'webci_db',
 'dbdriver' => 'mysqli',
 'dbprefix' => '',
 'pconnect' => FALSE,
 'db_debug' => (ENVIRONMENT !== 'production'),
 'cache_on' => FALSE,
 'cachedir' => '',
 'char_set' => 'utf8',
 'dbcollat' => 'utf8_general_ci',
 'swap_pre' => '',
 'encrypt' => FALSE,
 'compress' => FALSE,
 'stricton' => FALSE,
 'failover' => array(),
 'save_queries' => TRUE
);
```
'05. <br>
#--- Setting autoload.php - Framework CodeIgniter <br>
nano /var/www/webci/application/config/autoload.php <br>
```
$autoload['libraries'] = array('database');
```
'06. <br>
#--- Browsing Welcome Page - Framework Codeigniter <br>
links http://webci.idjvnix.com/ <br>
<br>
'07. <br>
#--- How To Install - Ion Auth for Codeigniter <br>
#--- Official Website : http://benedmunds.com/ion_auth/ <br>
#--- Download : https://github.com/benedmunds/CodeIgniter-Ion-Auth/zipball/3 <br>
#--- Webroot Location : /var/www/webci/ <br>
#--- URL Website : http://webci.idjvnix.com/ <br>
cd ~/ <br>
curl -LJO "https://github.com/benedmunds/CodeIgniter-Ion-Auth/zipball/3" <br>
unzip benedmunds-CodeIgniter-Ion-Auth-2.6.0-802-g149e300.zip <br>
<br>
cd ~/benedmunds-CodeIgniter-Ion-Auth-149e300/ <br>
cp config/ion_auth.php /var/www/webci/application/config/ <br>
cp controllers/Auth.php /var/www/webci/application/controllers/ <br>
cp language/english/* /var/www/webci/application/language/english/ <br>
cp libraries/Ion_auth.php /var/www/webci/application/libraries/ <br>
cp models/Ion_auth_model.php /var/www/webci/application/models/ <br>
cp -R views/auth/ /var/www/webci/application/views/ <br>
<br>
'08. <br>
#--- Generate Tables - Ion Auth for Codeigniter <br>
#--- Import SQL Scipt to Database <br>
mysql -u webuser -p webci_db < ~/benedmunds-CodeIgniter-Ion-Auth-149e300/sql/ion_auth.sql <br>
or <br>
mysql -u webuser -p <br>
```
mysql> use webci_db; 
mysql> SET autocommit=0 ; source ~/benedmunds-CodeIgniter-Ion-Auth-149e300/sql/ion_auth.sql ; COMMIT ;
```
'09. <br>
#--- Modif "Welcome.php" to Show Login Page <br>
nano /var/www/webci/application/controllers/Welcome.php <br> 
```
<?php
defined('BASEPATH') OR exit('No direct script access allowed');
class Welcome extends CI_Controller {
 public function __construct()
 {
  parent::__construct();
  $this->load->library('ion_auth');
  if (!$this->ion_auth->logged_in()) 
  {
   redirect('auth/login', 'refresh');
  }
 }
 public function index()
 {
  $this->load->view('welcome_message');
 }
}
```
'10. <br>
#--- Modif "welcome_message.php" to Show Logout Links <br>
nano /var/www/webci/application/views/welcome_message.php <br>
```
<?php
defined('BASEPATH') OR exit('No direct script access allowed');
?><!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="utf-8">
 <title>Welcome to CodeIgniter</title>
 <style type="text/css">
 ::selection { background-color: #E13300; color: white; }
 ::-moz-selection { background-color: #E13300; color: white; }
 body {
  background-color: #fff;
  margin: 40px;
  font: 13px/20px normal Helvetica, Arial, sans-serif;
  color: #4F5155;
 }
 a {
  color: #003399;
  background-color: transparent;
  font-weight: normal;
 }
 h1 {
  color: #444;
  background-color: transparent;
  border-bottom: 1px solid #D0D0D0;
  font-size: 19px;
  font-weight: normal;
  margin: 0 0 14px 0;
  padding: 14px 15px 10px 15px;
 }
 code {
  font-family: Consolas, Monaco, Courier New, Courier, monospace;
  font-size: 12px;
  background-color: #f9f9f9;
  border: 1px solid #D0D0D0;
  color: #002166;
  display: block;
  margin: 14px 0 14px 0;
  padding: 12px 10px 12px 10px;
 }
 #body {
  margin: 0 15px 0 15px;
 }
 p.footer {
  text-align: right;
  font-size: 11px;
  border-top: 1px solid #D0D0D0;
  line-height: 32px;
  padding: 0 10px 0 10px;
  margin: 20px 0 0 0;
 }
 #container {
  margin: 10px;
  border: 1px solid #D0D0D0;
  box-shadow: 0 0 8px #D0D0D0;
 }
 </style>
</head>
<body>
<div id="container">
   <a href="<?= base_url() ?>auth/logout">Logout</a>
 <h1>Welcome to CodeIgniter!</h1>
 <div id="body">
  <p>The page you are looking at is being generated dynamically by CodeIgniter.</p>
  <p>If you would like to edit this page you'll find it located at:</p>
  <code>application/views/welcome_message.php</code>
  <p>The corresponding controller for this page is found at:</p>
  <code>application/controllers/Welcome.php</code>
  <p>If you are exploring CodeIgniter for the very first time, you should start by reading the <a href="user_guide/">User Guide</a>.</p>
 </div>
 <p class="footer">Page rendered in <strong>{elapsed_time}</strong> seconds. <?php echo  (ENVIRONMENT === 'development') ?  'CodeIgniter Version <strong>' . CI_VERSION . '</strong>' : '' ?></p>
</div>
</body>
</html>
```
'11. <br>
#--- Browsing Website with Ion Auth Login Page <br>
links http://webci.idjvnix.com/ <br>
[done]. <br>
<br>
C. <br>
#--- Why Using FuelCMS (berbasis framework codeigniter) <br>  
'1. Security Feature, <br>
https://www.getfuelcms.com/features <br>
Set development passwords <br>
Restrict access by IP address <br>
Lock out users after custom number of failed attempts <br>
Native XSS and SQL injection protection <br>
Incorporated salted password hashing <br>
<br> 
'2. Responsive for security issues, <br>
https://www.cvedetails.com/product/49911/?q=Fuel+Cms <br>
https://github.com/daylightstudio/FUEL-CMS/issues/561 <br>
(FUEL CMS 1.4.8 allows SQL Injection) <br>
https://github.com/daylightstudio/FUEL-CMS/issues/478 <br>
(Vulnerability - Code Evaluations & SQL Injections - FUEL CMS 1.4.2) <br> 
<br>
#--- Refference FuelCMS, <br>
https://www.getfuelcms.com <br>
https://www.getfuelcms.com/features <br>
https://www.getfuelcms.com/faq <br>
https://www.getfuelcms.com/blog <br>
http://www.getfuelcms.com/user_guide/general/installing <br>
https://forum.getfuelcms.com <br>
https://docs.getfuelcms.com <br>
https://docs.getfuelcms.com/installation/requirements <br>
https://docs.getfuelcms.com/modules/blog <br>
https://docs.getfuelcms.com/modules/blog/themes <br>
https://github.com/daylightstudio/FUEL-CMS-Blog-Module <br>
https://www.getfuelcms.com/blog/2010/12/09/learning-fuel-cms-blog-series <br>
https://github.com/daylightstudio/FUEL-CMS <br>
https://github.com/daylightstudio/FUEL-CMS/issues <br>
https://www.cvedetails.com/product/49911/?q=Fuel+Cms <br>
<br>
D. <br> 
'01. <br>
#--- HowTo Install - FuelCMS <br> 
#--- Download : https://github.com/daylightstudio/FUEL-CMS/archive/master.zip <br>
#--- Webroot Location : /var/www/webcicms/ <br>
#--- URL Website : http://webcicms.idjvnix.com/ <br>
cd ~/ <br>
curl -LJO "https://github.com/daylightstudio/FUEL-CMS/archive/master.zip" <br>
unzip FUEL-CMS-master.zip <br>
mv ~/FUEL-CMS-master/* /var/www/webcicms <br>
mv ~/FUEL-CMS-master/.htaccess /var/www/webcicms <br>
<br>
'02. <br>
#--- Setting .htaccess  - FuelCMS <br> 
nano /var/www/webcicms/.htaccess <br>
```
Options +SymLinksIfOwnerMatch 
<IfModule mod_rewrite.c> 
 RewriteEngine On
 RewriteBase /
 <Files .*>
  Order Deny,Allow
  Deny From All
 </Files>
 # Allow asset folders through
 RewriteRule ^(fuel/modules/(.+)?/assets/(.+)) - [L]
 # Protect application and system files from being viewed
 RewriteRule ^(fuel/install/.+|fuel/crons/.+|fuel/data_backup/.+|fuel/codeigniter/.+|fuel/modules/.+|fuel/application/.+) - [F,L]
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule .* index.php/$0 [L]
 # Prevents access to dot files (.git, .htaccess) - security.
 RewriteCond %{SCRIPT_FILENAME} -d
 RewriteCond %{SCRIPT_FILENAME} -f
 RewriteRule "(^|/)\." - [F]
</IfModule>
Options -Indexes
```
'03. <br>
#--- Setting config.php - FuelCMS <br>
#--- Get "random encryption_key" using https://randomkeygen.com <br>
nano /var/www/webcicms/fuel/application/config/config.php <br>
```
$config['base_url'] = 'http://webcicms.idjvnix.com/'; 
$config['index_page'] = '';
$config['encryption_key'] = 'l>Njg9@+2<rQ@mc=(I>lV=H=4d}c>7';
```
'04. <br>
#--- Setting database.php - FuelCMS <br>
nano /var/www/webcicms/fuel/application/config/database.php <br>
```
$db['default'] = array(
 'dsn' => '',
 'hostname' => 'localhost',
 'username' => 'webuser',
 'password' => '1234567x',
 'database' => 'webcicms_db',
 'dbdriver' => 'mysqli',
 'dbprefix' => '',
 'pconnect' => FALSE,
 'db_debug' => (ENVIRONMENT !== 'production'),
 'cache_on' => FALSE,
 'cachedir' => '',
 'char_set' => 'utf8',
 'dbcollat' => 'utf8_general_ci',
 'swap_pre' => '',
 'encrypt' => FALSE,
 'compress' => FALSE,
 'stricton' => FALSE,
 'failover' => array(),
 'save_queries' => TRUE
);
```
'05 <br>
#--- Generate Tables - FuelCMS <br>
#--- Import SQL Scipt to Database <br>
mysql -u webuser -p webcicms_db < ~/var/www/webcicms/fuel/install/fuel_schema.sql <br> 
or <br>
mysql -u webuser -p <br> 
```
mysql> use webcicms_db; 
mysql> SET autocommit=0 ; source ~/var/www/webcicms/fuel/install/fuel_schema.sql ; COMMIT ;
```
'06. <br>
#--- Setting Ownership & Permission - FuelCMS <br>
chmod 777 /var/www/webcicms/fuel/application/cache/ <br>
chmod 777 /var/www/webcicms/fuel/application/cache/dwoo/ <br>
chmod 777 /var/www/webcicms/fuel/application/cache/dwoo/compiled/ <br>
chmod 777 /var/www/webcicms/assets/images/ <br>
<br>
'07. <br>
#--- setting MY_fuel.php - FuelCMS <br>
nano /var/www/webcicms/fuel/application/config/MY_fuel.php <br>
```
$config['site_name'] = 'CSIRT Pati'; 
$config['admin_enabled'] = TRUE; 
$config['fuel_mode'] = 'auto'; 
```
'08. <br>
#--- Browsing Welcome  Page - FuelCMS <br> 
http://webcicms.idjvnix.com/   (frontend) <br>
http://webcicms.idjvnix.com/fuel   (backend) <br>
[done]. <br>
<br>
E. <br>
#--- Why Using InfiniteBlog, <br>
https://codecanyon.net/item/infinite-blog-magazine-script/19890709 <br>
'1. Security Feature, <br>
Cross-Site Request Forgery (CSRF) Prevention <br>
Cross-Site Scripting (XSS) Prevention <br>
Password Hashing <br>
Avoiding SQL Injection <br>
<br>
'2. Frequently Update Version, <br>
22 August 2020 – Version 4.0 <br>
29 November 2019 – Version 3.9 <br>
01 April 2019 – Version 3.8.1 <br>
25 March 2019 – Version 3.8 <br>
16 July 2018 – Version 3.7 <br>
<br>
F. <br>
'01. <br>
#--- HowTo Install - InfiniteBlog (berbasis codeigniter) <br>
#--- Buy and Download : https://codecanyon.net/item/infinite-blog-magazine-script/19890709 <br>
#--- Webroot Location : /var/www/webcicms2/ <br>
#--- URL Website : http://webcicms2.idjvnix.com/ <br>
cd ~/ <br>
unzip infiniteblog-39.zip <br> 
mv ~/infiniteblog-39/codecanyon-19890709-infinite-blog-magazine-script/infinite-v3.9/* /var/www/webcicms2/ <br>
mv ~/infiniteblog-39/codecanyon-19890709-infinite-blog-magazine-script/infinite-v3.9/.htaccess /var/www/webcicms2/ <br>
<br>
'02. <br>
#--- Setting .htaccess - InfiniteBlog  <br>
nano /var/www/webcicms2/.htaccess <br>
```
RewriteEngine On 
RewriteCond %{REQUEST_FILENAME} !-f 
RewriteCond %{REQUEST_FILENAME} !-d 
RewriteRule ^(.*)$ index.php?/$1 [L] 
# Big File Uploads Setting 
php_value upload_max_filesize 32M 
php_value post_max_size 32M 
php_value memory_limit 256M 
```
'03. <br>
#--- Setting Ownership & Permission - InfiniteBlog <br>
chmod 777 /var/www/webcicms2/application/config/ <br>
chmod 777 /var/www/webcicms2/application/session/ <br>
chmod 777 /var/www/webcicms2/application/language/ <br>
chmod 777 /var/www/webcicms2/uploads/blocks/ <br>
chmod 777 /var/www/webcicms2/uploads/files/ <br>
chmod 777 /var/www/webcicms2/uploads/gallery/ <br>
chmod 777 /var/www/webcicms2/uploads/images/ <br>
chmod 777 /var/www/webcicms2/uploads/logo/ <br>
chmod 777 /var/www/webcicms2/uploads/profile/ <br>
chmod 777 /var/www/webcicms2/uploads/temp/ <br>
chmod 777 /var/www/webcicms2/application/config/database.php <br>
<br>
'04. <br>
#--- Create Database - InfiniteBlog <br>
mysql -u webuser -p 
```
mysql> SHOW DATABASES; <br>
mysql> CREATE webcicms2_db; <br>
```
'05. <br>
#--- Browsing http://webcms.idjvnix.com/install/ (to install InfiniteBlog) <br>
#--- set timezone = Asia/Jakarta <br>
links http://webcicms2.idjvnix.com/install/ <br>
<br>
'06. <br>
#--- Delete install folder, <br>
rm -rf /var/www/webcicms2/install/ <br>
<br>
'07. <br>
#--- Browsing Welcome Page - InfiniteBlog <br>
http://webcicms2.idjvnix.com/   (frontend) <br>
http://webcicms2.idjvnix.com/login   (backend) <br>
[done]. <br>
<br>
G. <br>
#--- Refference Framework & CMS <br> 
https://hotframeworks.com <br>
https://www.opensourcecms.com <br>
https://en.wikipedia.org/wiki/List_of_content_management_systems <br>
https://www.immuniweb.com/blog/top-10-most-popular-cms-under-attack-in-2018.html <br>
https://www.cvedetails.com/product/4096/?q=Wordpress <br>
https://www.cvedetails.com/product/16499/?q=Joomla%21 <br>
https://www.cvedetails.com/product/2387/?q=Drupal <br>
https://www.cvedetails.com/product/40837/?q=October+Cms <br>
https://www.cvedetails.com/product/7506/?q=Plone <br>
https://www.cvedetails.com/product/13267/?q=Serendipity+Freetag-plugin <br>
https://www.cvedetails.com/product/6858/?q=Typo3 <br>
<br>
Surabaya, 08/08/2020, <br>
Hope it can be useful, <br>
Best Regards, <br>
Bayu S. <br>
<br>
