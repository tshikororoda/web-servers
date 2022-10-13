# Web servers

|    |    TABLE OF CONTENTS                         |
|----|----------------------------------------------|
| 1. | [ Apache HTTP Server ](#apache)              |
| 2. | [ Glassfish ](#2)                            |
| 3. | [ Tomcat ](#3)                               |
| 4. | [ IIS (Internet Information Services) ](#4)  |

<a name="apache"></a>
## Apache HTTP Server
- [ Windows ](#windows)
- [ Linux   ](#linux)

<a name="windows"></a>
![Windows logo](./assets/windows-logo.png "The changes that has been made is successful.")

|    |                                                                                    |
|----|------------------------------------------------------------------------------------|
| 01 | [ Change _‘localhost’_ to a Domain Name in apache HTTP server ](#domain-name-w)    |
| 02 | [ Change document _root directory_ in apache HTTP server ](#change-document-root)  |
| 03 | [ How to run PHP as an Apache module ](#run-PHP-as-an-Apache-module)               |

<a name="domain-name-w"></a>
### Change _‘localhost’_ to a Domain Name In Apache HTTP Server
In this section, we want to change _localhost_ to access our webite/application. In this tutorial we will be using **`XAMPP`** package to install Apache distribution. XAMPP is the most popular PHP development environment. Installing XAMPP package allows us to develop, execute, test, and play around with _web server source code_ on our local machine before we deploy the _source code_ on the _live server_. We can configure and test out our website/application locally, instead of live web server somewhere. XAMPP is an easy to install Apache distribution containing **`MariaDB, PHP, and Perl`**. Just download and start the installer. It's that easy.

`To download XAMPP` [DOWNLOAD](https://www.apachefriends.org/download.html)

We can configure Apache Server in _XAMPP_ to serve up our web pages as though they were actually located on _http://www.our-web-site.com_ instead of **_localhost_**. <br />
This is the two steps process:

+ **First** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; : We have to redirect the _web site/application name (url)_ to our computer.
+ **Second** &nbsp; : Then, we get the Apache to redirect the _web site address_ to our _source code directory_.

##### Redirecting the Web Site name to Your Computer

When we type and search the web site/application name/address/url into the browser application URL field _(Let’s say you type: http://www.our-web-site.com)_, the browser sends the provided name or link to a _Domain Name Server_ that looks up for text name _http://www.our-web-site.com_ and returns the _Internet Protocol (IP)_ address for it. The browser connects to the _IP address_ it finds. <br />
Before our browser queries the _Domain Name Server_, it first checks a file called **_hosts_** on our computer. If the requested _web site name_ is found, it uses the IP address found in the file.

1. Let's Locate our hosts file.
> In Windows 10, this file located in  _**c:\windows\system32\drivers\etc**_

2. Editing host file
> Open the file with text editor _like Notepad++_. **_`Don’t use word processor because it can mess up the file when you save it`_**.

3. Add lines containing the domain name(s) and IP address to redirect to. <br />
Each line must begin with the IP address 127.0.0.1 _(this is the localhost IP address)_. After the _IP address_ follow the _name of the domain_ which we wish to _redirect_ to the _IP address_. **_We can only enter one name per line_**. Addresses names like _http://our-site.com and http://www.our-site.com_ are different and each need a separate line.
> 127.0.0.1 _[our-site.com](http://our-site.com " Dakalo Tshikororo")_<br />
> 127.0.0.1 _[www.our-site.com](http://www.our-site.com " Dakalo Tshikororo")_

##### Getting Apache to Serve our Web Site/application
1. Locate the _httpd-vhosts.conf_ file.
> _C:\xampp\apache\conf\extra:_

2. Open the file using a _plain text file editor_ like Notepad++.

```` html
<VirtualHost *:80>
    ServerAdmin codewithtshikororoda@our-site.com
    DocumentRoot "C:/workspace/dev-our-site.com"
    ServerName  our-site.com
    ServerAlias www.our-site.com

    # Other directives here
</VirtualHost>

````
> **ServerName**	: add the name of the domain <br />
> **ServerAlias**	: We can add as many alternate domains (separated by spaces) as you like that are supposed to resolve to this virtual host. **Remember that _http://www.our-domain-site.com_ is different from _http://our-domain-site.com_**, but we want them to be on the same web server. <br /> **DocumentRoot**	: add the path where our website/application _source code_ is located. We can use an absolute path  or a relative path.

_Save the file and restart the Apache Server for the changes to take effect. Do this by clicking on the Stop button on the XAMPP control panel and then clicking on the Start button. We could also just reboot the computer, but that is overkill_.

*` Last modified: 2022-06-14 time: 19:17PM `*

<a name="change-document-root"></a>
### Change document root directory In Apache HTTP Server
The _web directory_ is the home of all of our web based application's. `Document Root` on Apache web server is the _location or directory_ where to save files that are the source code of our web application.

Now, to change the _location_ of the `Document Root directory` to another document root configuration we have to open the file _`httpd.conf`_, usually the httpd.conf file located in the conf folder in Server Root directory. Since the _`Server Root`_ on my computer is _`C:/xampp/apache`_, the _`httpd.conf`_ file located in _`C:/xampp/apache/conf/httpd.conf`_.

1. Locate _`httpd.conf file`_: This will be _`C:\xampp\apache\conf`_
2. Open _`httpd.conf`_ to Edit using text editor <br />

  `Don’t use word processor because it can mess up the file when you save it`.<br />

  After _`httpd.conf`_ file is open, look for the string `DocumentRoot` on this file. We will find the configuration of Document Root in Apache web server. To change the location of the document root directory to another, `We simply replace the value of the document root configuration`.

  For instance: _the location of the document root on my computer is in `C:/XAMPP/htdocs`, then we want to change it to `c:/workspace`, then we simply replace the line `DocumentRoot "C:/XAMPP/htdocs" to DocumentRoot "c:/workspace"` and the line `<Directory "C:/xampp/htdocs"> to <Directory "c:/workspace">`._

````
DocumentRoot "C:/workspace"
<Directory "C:/workspace">
    #
    # Possible values for the Options directive are "None", "All",
    # or any combination of:
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    #
    # Note that "MultiViews" must be named *explicitly* --- "Options All"
    # doesn't give it to you.
    #
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.4/mod/core.html#options
    # for more information.
    #
    Options Indexes FollowSymLinks Includes ExecCGI

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   AllowOverride FileInfo AuthConfig Limit
    #
    AllowOverride All

    #
    # Controls who can get stuff from this server.
    #
    Require all granted
</Directory>

````

_After changes to the `httpd.conf` file is complete, save the file, then restart Apache. To test whether the change is successful, you can open a web browser and visit http://localhost._ If the results rendered by the web browser appears as shown in the image below, the changes made has been successful.

![New document root](./assets/successful.png "The changes that has been made is successful. ")

*` Last modified: 2022-06-16 time: 13:03PM `*

<a name="run-PHP-as-an-Apache-module"></a>
##### How to run PHP as an Apache module
##### PHP
In this tutorial, we'll be configuring PHP to run as an Apache module. However, PHP have *`built-in web server`* which can be launched by simply navigating into source code directory and run the PHP executable command with an -S parameter to set the localhost port.
```php
cd \workspace\test
php -S localhost:8001
```
PHP content file can be accessed in a browser at http://localhost:8001.

If we are `running multiple sites` using `PHP built-in web server`, we will have to specify different port for each site:
```
/site1 - > php -S localhost:8001
/site2 - > php -S localhost:8002
/site3 - > php -S localhost:8003
```

To Install PHP build on our system, is three steps process:

1. Step 1: Download PHP build Thread Safe(TS) zip package. This PHP build is for single process web servers.
   Windows PHP builds can be download from [PHP builds](https://www.php.net/downloads.php)

2. Step 2: Extract PHP build Thread Safe(TS) zip package
  PHP can be installed anywhere on our system. This means we can extract PHP build files at any directory of our choice. In this tutorial, we will extract PHP build files in `c:/Wamp/php`
  ```
    cd c:\
    mkdir Wamp
    cd wamp
    mkdir php
    cd php
  ```
3. Step 3: PHP build configuration setting may be set.

  The main PHP build configuration file is named `php.ini`. This file doesn’t exist initially, so we have to *`copy and paste`* php.ini-development and *rename* it to php.ini. This PHP’s configuration file contain default configuration which provides a development setup which reports all PHP errors and warnings. However, PHP does allow some setting to be set within a PHP script using method called ini_set().

  As we built our project, we will enable any required `extensions or PHP module`. This will depend on the libraries we need to use for functional requirements. The extensions below will aren't enabled by default. This will provides suitable development environment or production environment for most of our web applications:

```php
  extension=curl
  extension=gd
  extension=mbstring
  extension=pdo_mysql
```
To ensure Windows system can find the executable PHP file .exe, we need to add the PATH environment variable. Check [Microsoft documentation](#) on how to set system and user `“environment variable”`.
```
PATH: c:/Wamp/php
```
##### Apache
To install Apache, we download the latest Win64 ZIP file from [Apache lounge](https://www.apachelounge.com/download/) and extract it to any directory of our choice. Our directory will be. `c:\Wamp\Apache24`.

To configure PHP as an Apache HTTP server module, we ensure that Apache HTTP server isn’t running, if was started, we have to stop it from running. To perform this configuration we open and edit main Apache HTTP server configuration file called `httpd.conf`. According to our installation PATH in this tutorial, the location of this file is: c:\Apache24\conf\ httpd.conf. Add the following lines to the bottom of the file to set PHP as an Apache module:
```
# PHP7 module
# Setting PHP 7 module as an Apache module

PHPIniDir "c:/Wamp/php"
LoadModule php7_module "C:/php/php7apache2_4.dll"
AddType application/x-httpd-php .php

```

In the same file, also change the DirectoryIndex setting to load index.php instead of index.html when it can be found.
```
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>
```
When done editting, save `httpd.conf` file and test our configuration from a `cmd` command line. We use `httpd -t` command test if there is errors with our configuration. if `Syntax OK` displayed, it means configuration has been done correctly. If all went well, restart Apache with httpd. To execute `httpd command` any where on Windows system, Add `c:\Wamp\Apache24\bin` to the path environment.

*Open a cmd command prompt start Apache with:*

```
httpd -v
httpd -t
httpd -w

```
|          |                              |   
|----------|------------------------------|
| httpd -v | Version                      |
| httpd -t | Test if no errors            |
| httpd -w | Start the server             |
| CTRL + c | To stop server from running  |

##### Testing PHP file
Let's create a PHP file called `index.php` in Apache’s web directory root folder at C:\Wamp\Apache24\htdocs and add the following PHP source code:
```php
<!DOCTYPE html>
  <?php

  print phpinfo();

```
*Open a web browser and enter your server address: http://localhost/. A “PHP version” page will appear showing the various PHP and Apache configuration settings.*

*` Last modified: 2022-10-13 time: 12:09PM `*

---

<a name="linux"></a>
![Ubuntu](./assets/ubuntu-logo.png "The changes that has been made is successful.")

LAMP - stands for Linux, Apache, MySQL and PHP.

|    |                                                                                      |
|----|--------------------------------------------------------------------------------------|
| 01 | [ Install Apache HTTP server ](#install)                                             |
| 02 | [ Firewall configuration ](#firewall)                                                |
| 03 | [ Verifying Apache Web Server service ](#verify)                                     |
| 04 | [ Default welcome page ](#default-welcome-p)                                         |
| 05 | [ Change _‘localhost’_ to a Domain Name in apache HTTP server ](#domain-name-l)      |
| 06 | [ Change document _root directory_ in apache HTTP server ](#change-document-root-l)  |

<a name="install"></a>
### Install Apache HTTP server
We have to install Apache HTTP server in our host computer/virtual machine. The first thing we do is to update system repositories before we install apache, then we will install apache HTTP server.

*Update system repositories*
````
sudo apt-get update
````
*install apache HTTP server after update system repositories*
````
sudo apt-get install apache2
````
*` Last modified: 2022-09-10 time: 15:04PM `*

<a name="firewall"></a>
### Firewall configuration
Keep in mind that the only way we can access Apache from outside we must open specific port on our system. First, we have to enable UFW (Firewall). By default firewall (UFW) is disable. secondly we list the *application profiles* that we need to give Apache access to. Then we utilize/allow "Apache full" profile for enabling network activities on port 80. We then check the status which will show Apache allowed in firewall. Lastly, we Verifying Apache service is operational. Execute command below to make apache to accessible outside sequential:

*enable UFW*
````
sudo ufw enable
````
*list all application profile*
````
sudo ufw app list
````
*utilize/allow "Apache full"*
````
sudo ufw allow ‘Apache full’
````
<a name="verify"></a>
### Verifying Apache Web Server service
````
sudo systemctl status apache2
````
```js
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2021-02-24 20:39:39 UTC; 5 days ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 115 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)
    Process: 15247 ExecReload=/usr/sbin/apachectl graceful (code=exited, status=0/SUCCESS)
   Main PID: 128 (apache2)
      Tasks: 6 (limit: 4672)
     Memory: 16.4M
     CGroup: /system.slice/apache2.service
             ├─  128 /usr/sbin/apache2 -k start
             ├─15254 /usr/sbin/apache2 -k start
             ├─15255 /usr/sbin/apache2 -k start
             ├─15256 /usr/sbin/apache2 -k start
             ├─15257 /usr/sbin/apache2 -k start
             └─15258 /usr/sbin/apache2 -k start

Feb 27 00:00:23 ubuntu-db-mgmnt systemd[1]: Reloaded The Apache HTTP Server.
Feb 28 00:00:23 ubuntu-db-mgmnt systemd[1]: Reloading The Apache HTTP Server.
```
As confirmed by this output above, the service has started successfully. Another approach to verify if `Apache is running fine` is to request a web page from the Apache web server. To do so, find your IP address.
````
hostname -I
````
*` Last modified: 2022-09-10 time: 15:06PM `*

<a name="default-welcome-p"></a>
### Default welcome page
To see apache welcome page as show below, open the web browser. On the browser address bar, typein the IP address to access  or just type localhost. The page below indicates that Apache is working correctly. It also includes some basic information about important Apache files and directory locations.

![Default apache welcome page](./assets/default-welcome-page.PNG "The changes that has been made is successful. ")

*` Last modified: 2022-09-10 time: 2:52PM `*

<a name="domain-name-l"></a>
### Change _‘localhost’_ to a Domain Name In Apache HTTP Server

In this section, we want to change localhost to access our website/application. In this tutorial we will be using Apache. Apache is used to host PHP script. The process is similar for windows user as shown above with some minor changes. To install apache, open terminal and execute the command below. To open terminal you can simple press "CTRL + ALT + T".

We can configure Apache HTTP Server in Linux to serve up our web pages as though they were actually located on _http://www.our-web-site.com_ instead of **_localhost_**. This is the two steps process:

+ **First**    : We have to redirect the web site/application name (url) to our host computer.
+ **Second**   : Then, we get the Apache to redirect the web site address to our source code directory.

<a name=""></a>
##### Redirecting the Web Site name to Your Computer
When we type and search the web site/application name/address/url into the browser application URL field (Let’s say you type: http://www.our-web-site.com), the browser sends the provided name or link to a Domain Name Server that looks up for text name http://www.our-web-site.com and returns the Internet Protocol (IP) address for it. The browser connects to the IP address it finds.
Before our browser queries the Domain Name Server, it first checks a file called hosts on our computer. If the requested web site name is found, it uses the IP address found in the file.

1. Let's Locate hosts file on our host computer.
> In linux, this file located in  _**/etc/**_ <br />

2. Editing host file
> To edit file in linux we can use vim, nano or xed. Open the file with text editor _like vim, nano or exed, use the command below. **_`Don’t use word processor like open office because it can mess up the file when you save it`_**.
```html
sudo vim /etc/hosts
```
3. Add lines containing the domain name(s) and IP address to redirect to. Each line must begin with IP address 127.0.0.1 _(this is the localhost IP address)_. After _IP address_ follows the _name of domain_ which we wish to _redirect_ to the _IP address_. **_We can only enter one name per line_**.
> 127.0.0.1 _[our-site.com](http://our-site.com " Dakalo Tshikororo")_

#### Tips - Vim text editor
+ To insert text: press "i"
+ To save text/ new changes: press Esc, then :w
+ To quit/close vim: press :q
+ To save and close at the same time: :wq

````js
127.0.0.1       localhost
127.0.1.1       dev
127.0.0.1       our-site.com

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
````
*` Last modified: 2022-09-10 time: 15:08PM `*

<a name=""></a>
#### Getting Apache to Serve our Web Site/application
Apache on Linux (Ubuntu)has one server block enabled by default that is configured to serve documents from the **`/var/www/html`** directory. While this works well for a single site, it can become unwieldy if you are hosting multiple sites. Instead of modifying **`/var/www/html`**, we will create a directory structure within **`/var/www`** for _[our-site.com](http://our-site.com " Dakalo Tshikororo")_ site. This directory **`/var/www/html`** will remain in place as the default directory to be served if a client request doesn’t match any other domain sites in our apache web server. Next, assign ownership of the _[our-site.com](http://our-site.com " Dakalo Tshikororo")_ directory with **_`"www-data"`_** environment variable by utilize **`chown`** command. We then create a web page called **_`index.html`_** using `vim` text editor or any favorite text editor. Next, we set up virtual hosts file in `/etc/apache2/sites-available` and enable it, then we disable the default site defined in `000-default.conf`.

1. Create the directory in `"/var/www"` to store website data called. Execute the command below:
```
sudo mkdir -p /var/www/our-site.com
```

2. Change ownership of the directory with **_`"www-data"`_**.
```
sudo chown -R www-data:www-data /var/www/our-site.com
```
3. Create a web page called **_`index.html`_**.
````html
sudo vim /var/www/our-site.com/index.html
````
_Copy and paste `html code`_
  ```` html
  <!--/@tshikororoda
   Folder: hds/hds.index.html
   Document head and its related tags

  -->

  <!DOCTYPE html> <!-- HTML5 document version declaration -->
  <html dir="ltr" lang="en">
    <head>

  <!--____________________________________________
    [ Document header related tags here ]
  -->

      <meta charset     ="utf-8" />
      <meta http-equiv  ="X-UA-Compatible"  content="IE=edge" />
      <meta name        ="viewport"         content="width=device-width; initial-scale=1.0" />
      <meta name        ="author"           content="Dakalo Tshikororo" />

      <title> Document Appropriate Skeleton&#33; </title>

    </head>
    <body>

  <!--____________________________________________
    [ Document body related tags here ]
  -->

      <h1> &#39;Document Appropriate Skeleton&#33;&#39; </h1>
      <p> This is the document structure&#33;. </p>

    </body>
  </html>
  ````
4. Create a virtual hosts file and enable it.
```
sudo vim /etc/apache2/sites-available/our-site.com.conf
```
Paste in the following configuration block. Save and close the file when you are finished.
```` html
  <VirtualHost *:80>
      ServerAdmin  webmaster@our-site.com
      ServerName   our-site.com
      ServerAlias  www.our-site.com
      DocumentRoot /var/www/our-site.com
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
````
> **ServerName**	: add the name of the domain <br />
> **ServerAlias**	: We can add as many alternate domains (separated by spaces) as you like that are supposed to resolve to this virtual host. **Remember that _http://www.our-domain-site.com_ is different from _http://our-domain-site.com_**, but we want them to be on the same web server. <br /> **DocumentRoot**	: add the path where our website/application _source code_ is located. We can use an absolute path  or a relative path.

5. Enable the virtual hosts file for this site `our_site.com.conf`.
```
sudo a2ensite our_site.com.conf
```
6. Disable virtual hosts file for default site: `000-default.conf`.
```
sudo a2dissite 000-default.conf
```
7. Restart and test for configuration errors.
```
sudo systemctl restart apache2
sudo apache2ctl configtest
```
*` Last modified: 2022-09-10 time: 22:31PM `*

<a name="change-document-root-l"></a>
### Change **document root** directory In Apache HTTP Server

The web directory is the home of all of our web based application's. Document Root on Apache web server is the location or directory where source code files are saved of our web application. By default, Ubuntu doesn't allow access through the web browser to any file apart of those source code files located in `/var/www/html`. So if our web application is using a web document root located elsewhere, we are required to whitelist it. `/var/www/html` is a default document root.  

Now, to change the _location_ of the `Document Root directory` to another document root configuration we have to open the file _`apache2.conf`_, usually the apache2 file located in the apache2 folder in Server Root directory. Since the _`Server Root`_ on my computer is _`/etc/apache2`_, the _`apache2.conf`_ file located in _`/etc/apache2/apache2.conf`_.

1. Let's quickly Locate _`apache2.conf file`_: This will be _`C:/etc/apache2/apache2.conf`_
2. Open _`apache2.conf`_ to Edit using text editor <br />

  `Don’t use word processor because it can mess up the file when you save it`.<br />

  After _`apache2.conf`_ file is open, look for the string `Directory` on this file. We will find other or default configuration of Document Root in Apache web server. To change the location or to add new document root directory to another, `We simply place our configuration of the root we want here`.

```
<Directory /workspace/dev/>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

```
---
