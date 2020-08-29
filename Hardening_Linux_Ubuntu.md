## Hardening Linux Ubuntu 20.04 LAMP++ <br>
'01. <br>
#--- Donwnload & Install, The Latest Ubuntu Server 20.04 LTS + OpenSSH Server at,  <br>
https://ubuntu.com/download/server/ <br>
or <br>
http://old-releases.ubuntu.com/releases/ <br>
<br>
'02. <br>
#--- Prepare The Latest Ubuntu Tools and Essential tools, <br>
sudo apt update <br>
sudo apt list --upgradable <br>
sudo apt full-upgrade <br>
sudo apt clean <br>
sudo apt autoremove <br>
sudo apt autoclean <br>
<br>
sudo apt update <br>
sudo apt install aptitude <br>
sudo apt install gnupg apt-transport-https <br>
sudo apt install iputils-ping net-tools htop wget git unzip nano curl links elinks lynx python3 <br>
sudo apt-get install openssh-server openssh-client openssh-sftp-server <br>
sudo apt install vim-nox build-essential nmap <br>
<br>
'03. <br>
#--- Prepare The Right Timezone & Local Time, <br>
sudo apt install tzdata <br>
date <br>
<br>
sudo timedatectl list-timezones |grep Asia <br>
sudo timedatectl set-timezone Asia/Jakarta <br>
sudo timedatectl set-ntp on <br>
sudo systemctl restart systemd-timedated <br>
sudo timedatectl status <br>
date <br>
<br>
'04. <br>
#--- Prepare The Right Hostname & Domain Name, <br>
sudo nano /etc/hosts <br>
```
127.0.0.1       localhost 
#--- Change the setting below to suit your condition 
51.79.165.182     csirt.idjvnix.com     csirt 
```
sudo hostnamectl set-hostname csirt.idjvnix.com <br>
sudo hostnamectl status <br>
sudo nano /etc/hostname <br>
```
csirt.idjvnix.com 
```
sudo hostname csirt.idjvnix.com <br>
hostname <br>
```
csirt.idjvnix.com 
```
'05. <br>
#--- Prepare Sudoers User (Admin User), <br>
sudo adduser usertrial <br>
sudo usermod -aG sudo usertrial <br>
sudo usermod -aG www-data usertrial <br>
<br>
'06. <br>
#--- Securing Service SSH (change SSH Port), <br>
sudo nano /etc/ssh/sshd_config <br>
```
Port 1974
sudo systemctl restart sshd
sudo systemctl status sshd
```
sudo netstat -plnt <br>
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:1974            0.0.0.0:*               LISTEN      29667/sshd: /usr/sb
tcp6       0      0 :::1974                 :::*                    LISTEN      29667/sshd: /usr/sb
```
#--- Restart Ubuntu Server <br>
sudo reboot <br>
<br>
'07. <br>
#--- Install & Config Service Apache2 (Web Server) and Securing Apache2, <br>
sudo apt update <br>
sudo apt install apache2 <br>
sudo systemctl enable apache2 <br>
sudo systemctl restart apache2 <br>
sudo systemctl status apache2 <br>
<br>
#--- How To Check Apache2 version, <br>
apache2 -v <br>
or <br>
apachectl -v <br>
<br>
sudo chown -R www-data:www-data /var/www/html <br>
sudo chmod 775 /var/www/html <br>
<br>
#--- Create SSL csirt.idjvnix.com using https://www.sslforfree.com/ <br>
#--- Verification Method for csirt.idjvnix.com : HTTP File Upload <br>
#--- Upload the Auth File to your HTTP server under: /.well-known/pki-validation/ <br>
unzip csirt.idjvnix.com.zip -d csirt.idjvnix.com <br>
sudo cp ~/csirt.idjvnix.com/* /etc/ssl/ <br>
<br>
ls -lh /etc/ssl/ <br>
```
total 44K
-rw-r--r-- 1 root root     2.4K Aug  3 12:46 ca_bundle.crt
-rw-r--r-- 1 root root     2.3K Aug  3 12:46 certificate.crt
drwxr-xr-x 2 root root      16K Aug  3 10:35 certs
-rw-r--r-- 1 root root      11K Apr 20 18:53 openssl.cnf
drwx--x--- 2 root ssl-cert 4.0K Aug  3 10:35 private
-rw-r--r-- 1 root root     1.7K Aug  3 12:46 private.key
```
sudo nano /etc/apache2/sites-available/000-default.conf
```
<VirtualHost *:80>
        ServerName csirt.idjvnix.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
        <Directory /var/www/html>
             Options Indexes FollowSymLinks MultiViews
             AllowOverride All
             Require all granted
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<VirtualHost *:443>
        ServerName csirt.idjvnix.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
        <Directory /var/www/html>
             Options Indexes FollowSymLinks MultiViews
             AllowOverride All
             Require all granted
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        SSLEngine On
        SSLCertificateFile /etc/ssl/certificate.crt
        SSLCertificateKeyFile /etc/ssl/private.key
        SSLCertificateChainFile /etc/ssl/ca_bundle.crt
</VirtualHost>

sudo nano /etc/apache2/sites-available/default-ssl.conf
<IfModule mod_ssl.c>
        <VirtualHost _default_:443>
                ServerAdmin webmaster@localhost
                DocumentRoot /var/www/html
                ErrorLog ${APACHE_LOG_DIR}/error.log
                CustomLog ${APACHE_LOG_DIR}/access.log combined
                SSLEngine on
                SSLCertificateFile /etc/ssl/certificate.crt
                SSLCertificateKeyFile /etc/ssl/private.key
                SSLCertificateChainFile /etc/ssl/ca_bundle.crt
                <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                SSLOptions +StdEnvVars
                </FilesMatch>
                <Directory /usr/lib/cgi-bin>
                                SSLOptions +StdEnvVars
                </Directory>
        </VirtualHost>
</IfModule>
```
sudo a2enmod ssl <br>
sudo systemctl reload apache2 <br>
sudo systemctl restart apache2 <br>
sudo systemctl status apache2 <br>
<br>
#--- Notes - How To Check Valid SSL <br>
openssl s_client -CApath /etc/ssl/ -connect csirt.idjvnix.com:443 <br>
<br>
#--- Enable mod_rewrite to activating rules on .htaccess, <br>
sudo nano /etc/apache2/apache2.conf <br>
```
Mutex file:${APACHE_LOCK_DIR} default
Timeout 300
KeepAlive On
MaxKeepAliveRequests 100
KeepAliveTimeout 5
User ${APACHE_RUN_USER}
Group ${APACHE_RUN_GROUP}
HostnameLookups Off
ErrorLog ${APACHE_LOG_DIR}/error.log
LogLevel warn
IncludeOptional mods-enabled/*.load
IncludeOptional mods-enabled/*.conf
Include ports.conf
<Directory />
        Options FollowSymLinks
        AllowOverride None
        Require all denied
</Directory>
<Directory /usr/share>
        AllowOverride None
        Require all granted
</Directory>
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
<Directory /var/www/html/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
AccessFileName .htaccess
<FilesMatch "^\.ht">
        Require all denied
</FilesMatch>
LogFormat "%v:%p %h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" vhost_combined
LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%h %l %u %t \"%r\" %>s %O" common
LogFormat "%{Referer}i -> %U" referer
LogFormat "%{User-agent}i" agent
IncludeOptional conf-enabled/*.conf
IncludeOptional sites-enabled/*.conf
```
sudo a2enmod rewrite <br>
sudo systemctl restart apache2 <br>
sudo systemctl status apache2 <br>
<br>
sudo netstat -plnt <br>
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:1974            0.0.0.0:*               LISTEN      19741/sshd
tcp6       0      0 :::80                   :::*                    LISTEN      21755/apache2
tcp6       0      0 :::1974                 :::*                    LISTEN      19741/sshd
tcp6       0      0 :::443                  :::*                    LISTEN      21755/apache2
```
'08. <br>
#-- Install & Config PHP Scripting Programming, <br>
sudo apt update <br>
sudo apt install php7.4 php7.4-curl php7.4-dom php7.4-gd php7.4-intl <br>
sudo apt install php7.4-mbstring php7.4-mcrypt php7.4-xml php7.4-xsl php7.4-zip <br>
sudo systemctl restart apache2 <br>
sudo systemctl status apache2 <br>
<br>
#--- If you want to run multiple PHP (php7.4 & php5.6), follow this step, <br>
sudo apt update <br>
sudo apt  install software-properties-common <br>
sudo add-apt-repository ppa:ondrej/php <br>
sudo apt update <br>
sudo apt install php5.6 <br>
sudo a2dismod php7.4 <br>
sudo a2enmod php5.6 <br>
sudo systemctl restart apache2 <br>
sudo systemctl status apache2 <br>
<br>
#--- How To Change Version of PHP using, <br>
sudo update-alternatives --config php <br>
<br>
#--- How To Check PHP version, <br>
php -v <br>
<br>
#--- How To Check All Apache & PHP Info, <br>
nano /var/www/html/cekphpinfoku.php <br>
```
<?php phpinfo(); ?>
```
#--- Browsing PHP Info Scripts, <br>
links http://localhost/cekphpinfoku.php <br>
<br>
'09. <br>
#--- Install & Config proftpd (FTP Server, Standalone Mode), <br>
sudo apt update <br>
sudo apt install proftpd <br>
<br>
#--- Config proftpd - FTP Server <br>
sudo nano /etc/proftpd/proftpd.conf <br>
```
Include /etc/proftpd/modules.conf
IdentLookups                    off
ServerName                      "Debian"
ServerType                      standalone
DeferWelcome                    off
MultilineRFC2228                on
DefaultServer                   on
ShowSymlinks                    on
TimeoutNoTransfer               600
TimeoutStalled                  600
TimeoutIdle                     1200
DisplayLogin                    welcome.msg
DisplayChdir                    .message true
ListOptions                     "-lah"
DenyFilter                      \*.*/
Port                            21
MaxInstances                    30
User                            proftpd
Group                           nogroup
Umask                           022  022
AllowOverwrite                  on
TransferLog /var/log/proftpd/xferlog
SystemLog   /var/log/proftpd/proftpd.log
UseLastlog on
SetEnv TZ :/etc/localtime
<IfModule mod_quotatab.c>
QuotaEngine off
</IfModule>
<IfModule mod_ratio.c>
Ratios off
</IfModule>
<IfModule mod_delay.c>
DelayEngine on
</IfModule>
<IfModule mod_ctrls.c>
ControlsEngine        off
ControlsMaxClients    2
ControlsLog           /var/log/proftpd/controls.log
ControlsInterval      5
ControlsSocket        /var/run/proftpd/proftpd.sock
</IfModule>
<IfModule mod_ctrls_admin.c>
AdminControlsEngine off
</IfModule>
Include /etc/proftpd/tls.conf
```
#--- Config proftpd - FTP Server (TLS Connections), <br>
sudo nano /etc/proftpd/tls.conf <br>
```
<IfModule mod_tls.c>
TLSEngine                               on
TLSLog                                  /var/log/proftpd/tls.log
TLSProtocol                             SSLv23
TLSRSACertificateFile                   /etc/ssl/certificate.crt
TLSRSACertificateKeyFile             /etc/ssl/private.key
TLSOptions                      NoCertRequest EnableDiags NoSessionReuseRequired
TLSVerifyClient                         off
TLSRequired                             on
</IfModule>
```
#--- Restart proftpd - FTP Server <br>
sudo systemctl enable proftpd <br>
sudo systemctl restart proftpd <br>
sudo systemctl status proftpd <br>
<br>
sudo netstat -plnt <br>
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:1974            0.0.0.0:*               LISTEN      1384/sshd
tcp6       0      0 :::80                   :::*                    LISTEN      15242/apache2
tcp6       0      0 :::21                   :::*                    LISTEN      16800/proftpd: (acc
tcp6       0      0 :::1974                 :::*                    LISTEN      1384/sshd
tcp6       0      0 :::443                  :::*                    LISTEN      15242/apache2
```
'10. <br>
#--- Install & Config mySQL (Database Server), <br>
sudo apt update <br>
sudo apt install mysql-server-5.7 mysql-client-5.7 php5.6-mysql <br>
<br>
#--- Check mySQL version <br>
mysql -V <br>
<br>
#--- Try to connect mySQL server <br>
mysql -u root -p <br>
```
mysql> CREATE USER usertrial@localhost IDENTIFIED BY '1234567x';
mysql> GRANT ALL PRIVILEGES ON * . * TO 'usertrial'@'localhost';
mysql> FLUSH PRIVILEGES;
mysql> SHOW GRANTS;
mysql> SELECT user, host FROM user;
mysql> select * from user where USER='usertrial';
```
#--- Install PHPMyadmin (MySQL web administration tool, Using Apache2), <br>
sudo apt update <br>
sudo apt install phpmyadmin <br>
<br>
sudo systemctl enable mysql <br>
sudo systemctl restart mysql <br>
sudo systemctl status mysql <br>
<br>
sudo systemctl restart apache2 <br>
sudo systemctl status apache2 <br>
<br>
sudo netstat -plnt <br>
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      22989/mysqld
tcp        0      0 0.0.0.0:1974            0.0.0.0:*               LISTEN      1384/sshd
tcp6       0      0 :::80                   :::*                    LISTEN      23073/apache2
tcp6       0      0 :::21                   :::*                    LISTEN      16800/proftpd: (acc
tcp6       0      0 :::1974                 :::*                    LISTEN      1384/sshd
tcp6       0      0 :::443                  :::*                    LISTEN      23073/apache2
```
'11. <br>
#--- Install & Config NodeJs Scripting Programming, <br>
#--- Downloading nvm from git & Cloning into '~/.nvm' <br>
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash <br>
<br>
#--- export Bash Environment Variable <br>
export NVM_DIR="$HOME/.nvm" <br>
<br>
#--- This loads nvm <br>
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" <br>
<br>
#--- This loads nvm bash_completion <br>
[ -s "$NVM_DIR/bash_completion" ] && . "$NVM_DIR/bash_completion" <br>
<br>
#--- Install Nodejs Using NVM <br>
command -v nvm <br>
nvm ls-remote <br>
nvm install v14.8.0 <br>
node --version <br>
<br>
#--- Create Nodejs Web Application Server, <br>
nano app.js <br>
```
const http = require('http'); 
const hostname = '0.0.0.0'; 
const port = 3000; 
const server = http.createServer((req, res) => { 
  res.statusCode = 200; 
  res.setHeader('Content-Type', 'text/plain'); 
  res.end('Hello World'); 
}); 
server.listen(port, hostname, () => { 
  console.log(`Server running at http://${hostname}:${port}/`);
}); 
```
#--- Run Nodejs Web Application Server (Use Ctrl + c to Stop It), <br>
node app.js <br>
```
Server running at http://0.0.0.0:3000/
```
#--- Browse Nodejs Web Application Server (Using Other Terminals), <br>
links http://localhost:3000 <br>
<br>
[end]. <br>
<br>
Surabaya, 08/08/2020, <br>
Hope it can be useful, <br>
Best Regards, <br>
Bayu S. <br>
<br>
