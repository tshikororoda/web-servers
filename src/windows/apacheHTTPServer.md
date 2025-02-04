# Apache `HTTP` server
![Windows logo](../assets/windows-logo.png "The changes that has been made is successful.")
***
<!--

  DN  - domain name
-->

## Table of Contents

+ [ How to run _PHP_ as an _Apache module_](#PHPasApachemodule)
  + [PHP built-in web server](#builtinserver)
  + [PHP build Thread Safe(TS)](#PHPbuild)
  + [HTTP apache lounge](#apachelounge)
    + [Installing PHP as an Apache handler](#PHPasanApachehandler)
    + [Testing PHP file](#testphpfile)
    + [Configure Apache to run PHP as FastCGI](#PHPasFastCGI)
    + [Testing PHP file](#testphpfile)
+ [Setup multiple PHP build versions](#multiplePHPbuild)
  + [Download TS *(thread safe)* version](#downloadtsv)
  + [Configure php.ini](#configphp.ini)
  + [Apache configuration](#apacheconfiguration)
    + [Setup default PHP build version](#defaultPHPbuild)
    + [Fast CGI](#FastCGI)
    + [Configure to use PHP build 5.6.9v](#usePHPbuild5.6.9v)
    + [Setup virtual host)](#svhosts)

***

<a name="PHPasApachemodule"></a>
### `Apache HTTP Server:` Run PHP as an Apache module

+ [PHP built-in web server](#builtinserver)
+ [PHP build Thread Safe(TS)](#PHPbuild)
+ [HTTP apache lounge](#apachelounge)
  + [Installing PHP as an Apache handler](#PHPasanApachehandler)
  + [Testing PHP file](#testphpfile)
  + [Configure Apache to run PHP as FastCGI](#PHPasFastCGI)
  + [Testing PHP file](#testphpfile)

<a name="builtinserver"></a>
###### `PHP:` Built-in web server
*In this section*, we'll be configuring PHP to run as an `Apache module`. However, PHP have *`built-in web server`* which can be launched by simply navigating into source code directory and run the *PHP executable command* with an `-S parameter` to set the localhost and port number.

```sh

mkdir dev-PHPBuiltInWebServer
cd dev-PHPBuiltInWebServer

# Start php built-in web server
php -S localhost:8001

```
PHP content file can be accessed in a browser at [*http://localhost:8001*]( http://localhost:8001)

If we are `running multiple sites` using `PHP built-in web server`, we will have to specify different port for each site:

```sh

  php -S localhost:8001 # /site1
  php -S localhost:8002 # /site2
  php -S localhost:8003 # /site3

```
<a name="PHPbuild"></a>
###### `PHP:` Build Thread Safe(TS)
To Install PHP build Thread Safe(TS) on our system: three steps process:

+ `Step 1:` Download `PHP build Thread Safe (TS) zip package`. This PHP build is for single process web servers. Windows PHP builds can be download from [PHP builds](https://www.php.net/downloads.php)

+ `Step 2:` Extract PHP build Thread Safe (TS) zip package. PHP can be installed anywhere on our system. This means we can extract PHP build files at any directory of our choice. In this section, we will extract PHP build files in `c:/devtools/php@8.3.2`

  ```sh
  # 8.3.2 is php version
  #path: c:/devtools/php@8.3.2

  mkdir php@8.3.2
  cd php@8.3.2

  ```

+ `Step 3:` PHP build configuration setting.

  The main PHP build configuration file is named `php.ini`. This file doesn’t exist initially, so we have to *copy and paste* `php.ini-development` and *rename* it to `php.ini`. This PHP’s configuration file contain default configuration which provides a development setup which reports all PHP errors and warnings. However, PHP does allow some setting to be set within a PHP script using method called `ini_set()`.

  As we built our project, we will enable any required `php extensions / module`. This will depend on the libraries we need to use for `functional requirements`. The extensions below aren't enabled by default. This will provides suitable development environment or production environment for most of our web applications:

  ##### *configure php.ini*
  *Now* open `php.ini` file and remove the semicolon `;` from the `extension_dir = "ext"` setting.
  _Usually_, we would want to have `cURL`, `gd`, `mbstring`, `fileinfo`, and `pdo_sqlite` enabled. Search and remove the semicolon `;` for any extensions we need to start.

  ```sh
    # remove ; )

    extension_dir = "ext"

    extension=curl
    extension=gd
    extension=fileinfo
    extension=mbstring
    extension=pdo_mysql

  ```

##### `PHP:` System variable
To ensure Windows system can find the executable PHP file .exe, we need to add the PATH environment variable: `PATH: c:/devtools/php@8.3.2`.
+ In Search, search for and then select: System (Control Panel)
+ Click the *`Advanced system settings`* link.
+ Click Environment Variables. In the section *`System Variables`* find the PATH environment variable and select it. Click *`Edit`*. If the PATH environment variable does not exist, *`click New`*.
+ In the *`Edit System Variable (/ New System Variable)`* window, specify the value of the PATH environment variable. Click OK. Close all remaining windows by clicking *`OK`*.
+ Re-open *command prompt* window, and *run* PHP executable *command* with `-v parameter` below that allows us to check the *php version* install.

```sh
  # check php version installed
  php -v

```

<a name="apachelounge"></a>
##### HTTP Apache lounge
To install HTTP Apache, we download the latest Win64 ZIP file from [Apache lounge](https://www.apachelounge.com/download/) and *extract* it to any directory of our choice. Our directory will be. `c:\devtools\apache`.

There are three ways to set up PHP to work with Apache 2.x on Windows. PHP can be run as a: `handler`, as a `CGI`, or under `FastCGI`.

To configure PHP as an Apache HTTP server module, we ensure that Apache HTTP server isn’t running, if was started, we have to stop it from running. To perform this configuration we open and edit main Apache HTTP server configuration file called `httpd.conf`. According to our installation PATH in this section, the location of this file is: `c:\devtools\apache\conf\httpd.conf`. Add the following lines to the bottom of the `Apache httpd.conf configuration` file to set PHP as an Apache module.

###### Installing PHP as an Apache handler.

```sh
# The name of the module for PHP 8.x.x is php8apache2_4 module
# For PHP 7.X.X the module is php7apache2_4 module

# LoadModule php7_module "c:/devtools/php@7.x/Win64/php7apache2_4.dll"
LoadModule php_module "c:/devtools/php@8.4.3/php8apache2_4.dll"

# AddType can also be used in place of using FilesMatch with SetHandler
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>

```

In the same file, also change the `DirectoryIndex` setting to load `index.php` instead of `index.html` when it can be found.

```sh

<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

```

When done editing, save `httpd.conf` file changes and then test our configuration from a `cmd` command line. We use `httpd -t` command test if there is errors with our configuration. if `Syntax OK` displayed, it means configuration has been done correctly. If all went well, restart Apache with httpd. To execute `httpd command` any where on Windows system, add `c:\devtools\Apache24\bin` to the system path environment variable.

*Open command prompt to start Apache with:*

```sh

httpd -v
httpd -t
httpd -w

```
<a name="testphpfile"></a>
##### Testing PHP document
Let's create a PHP file called `index.php` in Apache’s web directory root folder at `c:\devtools\apache\htdocs` and add the following PHP source code:

###### Add _phpinfo()_ on index.php
```sh

<?php

echo phpinfo();

```
![phpinfo() 8.1.12 version](../assets/8112.jpg "HTTP Apache lounge is successful installed.")

###### Configure Apache to run PHP as FastCGI

So, to run PHP as `Fast CGI server API` instead of `Apache 2.0 handler`. We download [*mod_fcgid-2.3.10-win64-VS16.zip*](https://www.apachelounge.com/download/) based on our apache architecture. Once downloaded, extract the zip file and copy `mod_fcgid.so` file to `c:\devtools\apache\modules` directory. And enable `fcgid_module` on the main Apache HTTP server configuration file called `httpd.conf` as shown below. Search and remove the Pound sign #

```sh

# Enable fcgid_module
LoadModule fcgid_module modules/mod_fcgid.so

# Configure the path to php.ini
FcgidInitialEnv PHPRC "c:/devtools/php@8.3.2"
<FilesMatch \.php$>
    SetHandler fcgid-script
</FilesMatch>

FcgidWrapper "c:/devtools/php@8.3.2/php-cgi.exe" .php

```
In the same file `httpd.conf`, also add `Includes ExecCGI` within `<Directory "${SRVROOT}/htdocs">` to CGI allow execution to avoid getting Forbidden error (403) when you render the test page. We can also do this on the virtual hosts _(vhosts)_ file for specific directory.

```sh

<Directory "${SRVROOT}/htdocs">
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
    AllowOverride None

    #
    # Controls who can get stuff from this server.
    #
    Require all granted
    #
</Directory>

```

In the same file, also change the `DirectoryIndex` setting to load `index.php` instead of `index.html` when it can be found.

```sh

<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

```

When done editing, save `httpd.conf` file changes and then test our configuration from a `cmd` command line. We use `httpd -t` command test if there is errors with our configuration. if `Syntax OK` displayed, it means configuration has been done correctly. If all went well, restart Apache with httpd. To execute `httpd command` any where on Windows system, add `c:\devtools\Apache24\bin` to the system path environment variable.

*Open command prompt to start Apache with:*

```sh

httpd -v
httpd -t
httpd -w

```
| Command  |    Definition                |   
|----------|------------------------------|
| httpd -v | Version                      |
| httpd -t | Test if no errors            |
| httpd -w | Start the server             |
| CTRL + c | To stop server from running  |

<a name="testphpfile"></a>
##### Testing PHP document
Let's create a PHP file called `index.php` in Apache’s web directory root folder at `c:\devtools\apache\htdocs` and add the following PHP source code:

###### Add phpinfo() on index.php
```sh
<?php

echo phpinfo();

```
![phpinfo() 8.1.12 version](../assets/8112.jpg "HTTP Apache lounge is successful installed.")

*Open a web browser and enter your server address: [http://localhost:8080](). A PHP version page will appear showing the various PHP and Apache configuration settings.*

*` Last modified: 2023-07-08 time: 16:38PM `*

<a name="multiplePHPbuild"></a>
### `Apache HTTP Server:` Setup multiple PHP build versions
+ [Download TS *(thread safe)* version](#downloadtsv)
+ [Configure php.ini](#configphp.ini)
+ [Apache configuration](#apacheconfiguration)

<a name="downloadtsv"></a>
##### `PHP:` Download TS (thread safe) version
In this section, we will install two different versions of PHP build. PHP 8.4.3 build has already been installed on our previous section above where we configer php build thread safe (TS) to run as a module. <a name="downloadtsv"></a>*Firstly,* we’ll need to [*download TS (thread safe) version*](https://windows.php.net/downloads/releases/archives/) to install PHP 5.6.9 TS build to work along-side with PHP 8.4.3 TS build. Each PHP version build installed will be configured *per-project directory requirements*.

Before downloading PHP build version, let's check if we have Thread Safety enabled in our current *PHP* installation by executing `phpinfo()`
build-in function inside our `C:\devtools\apache\htdocs\index.php` directory as shown on the previous section above. *Now* search for `Thread Safety` and *verify* if we have enabled it or not. *If we have Thread Safety enabled, then we should download the PHP build version that contain Thread Safety in it's name. And opposite will be the case with,
if we have Non-Thread Safety enabled, you should download the version that contains Non-Thread Safety in it's name.*

*Once* the [download finishes](https://windows.php.net/downloads/releases/archives/), we go to `C:\devtools\php` directory and create a new PHP build TS folder called `php@5.6.9`. *Then*, we open the archive PHP build TS _(thread safe)_ version we’ve downloaded and *extract* everything in this folder: php@5.6.9.

```sh
# Path C:\devtools
# Create directory to extract PHP build TS (thread safe).

mkdir php@5.6.9

```

<a name="configphp.ini"></a>
##### `PHP:` *configure php.ini*
In the same directory 5x, locate the `php.ini-development` file. _Copy, paste and rename_ it to php.ini. Open the `php.ini` file and remove the semicolon `;` from the `extension_dir = "ext"` setting.
_Usually_, we would want to have `cURL`, `FTP`, `fileinfo`, `MySQL`, `MySQLi`, `openssl`, and `pdo_sqlite` enabled. Search and remove the semicolon `;` for any extensions we need to start.

```sh
# remove ;

extension_dir = "ext"

extension=curl
extension=gd
extension=fileinfo
extension=mbstring
extension=pdo_mysq

```

<a name="apacheconfiguration"></a>
#### Apache configuration
+ [Setup default PHP build version](#defaultPHPbuild)
+ [Fast CGI](#FastCGI)
+ [Configure to use PHP build 5.6.9v](#usePHPbuild5.6.9v)
+ [Setup virtual host)](#svhosts)

<a name="defaultPHPbuild"></a>
##### `HTTP apache server:` Set default PHP build TS version
Everything looks good, it’s time to configure our default `PHP 8.1.12 build`.  Go to `C:\devtools\apache\conf` to locate file named `httpd.conf`. Enable Various default settings. It's not enable by default. Search on the server config file and remove # as shown below.

```sh
# Remove # )
Include conf/extra/httpd-default.conf

```

Now, go to `C:\devtools\apache\conf\extra` to locate file named `httpd-default.conf`. Preferably at the end of  `httpd-default.conf` file. We can install default PHP either as an Apache handler or install PHP as fcgi module. Add the following chunk of source code below:

```sh
# Installing PHP as an Apache handler.
# The name of the module for PHP 8.x.x is php_module.
# For PHP 7.X.X the module is php7_module.

LoadModule php_module "c:/devtools/php@8.3.2/php8apache2_4.dll"
# LoadModule php7_module "c:/devtools/php/7.x/Win64/php7apache2_4.dll"
AddType application/x-httpd-php .php

# FilesMatch with SetHandler can also be used in place of using AddType
#
# <FilesMatch \.php$>
#    SetHandler application/x-httpd-php
# </FilesMatch>

# configure the path to php.ini
PHPIniDir "c:/devtools/php@8.3.2"

```
*Open command prompt to stop, test and start Apache to see if changes are successful:*

```sh
# To stop apache server press CTRL + c
httpd -t
httpd -w

```
<a name="FastCGI"></a>
#### Fast-CGI
#####  `HTTP apache server:` Download FastCGI
Apache in itself can’t run multiple PHP build TS versions. So, what we’re going to do is update it to use `Fast CGI server API` instead of `Apache 2.0 handler`.

We download [*`mod_fcgid-2.3.10-win64-VS16.zip`*](https://www.apachelounge.com/download/) based on our HTTP apache server architecture. Once downloaded, extract the zip file and copy `mod_fcgid.so` file to `c:\devtools\apache\modules` directory. And enable fcgid_module on the main Apache HTTP server configuration file called `httpd.conf` as shown below. Search and remove the Pound sign #

```sh
# Remove # or add it if isn't available.
LoadModule fcgid_module modules/mod_fcgid.so

```
###### `HTTP apache server:` Configure apache to use PHP build (thread safe) version 5.6.9
*Now*, to let Apache know that we have a new PHP build _(thread safe)_ version 5.6.9 version ready to be used. We open and edit main HTTP Apache server configuration file called `httpd.conf`. According to our installation PATH in this section, the location of this file is: `c:\devtools\apache\conf\httpd.conf`. _Add_ the source code bellow to the bottom of the file. This will tell HTTP Apache server where to find our specific PHP build _(thread safe)_ version.

```sh

# Configure Apache to run PHP as FastCGI
# PHP 5.6.9
# Not default PHP build (thread safe) version in this case.

ScriptAlias /php5.6.9 "C:/devtools/php@5.6.9"
Action application/x-httpd-php5.6.9-cgi /php5.6.9/php-cgi.exe

<Directory "C:/devtools/php/5.6.9">
    AllowOverride None
    Options None
    Require all denied

    <Files "php-cgi.exe">
        Require all granted
    </Files>

    SetEnv PHPRC "C:/devtools/php@5.6.9"
</Directory>

```

<a name="usePHPbuild5.6.9v"></a>
#### Testing PHP build TS 5.6.9

Now, in every project directory that we want to use PHP build TS version 5.6.9 we must include the code below on our virtualhost configuration on virtual host: `c:\devtools\apache\conf\extra\httpd-vhosts.conf`.
```sh

UnsetEnv PHPRC

<FilesMatch "\.php$">
    SetHandler application/x-httpd-php5.6.9-cgi
</FilesMatch>

```

<a name="svhosts"></a>
#### Setting up virtual host

`httpd-vhosts.conf` is not it's not enable by default. Go to `C:\devtools\apache\conf` to locate file named `httpd.conf`. Enable virtual host module _(mod_vhost_alias.so)_ by search 'Include conf/extra/httpd-vhosts.conf' statements and remove # as shown below:

```sh

# Virtual hosts virtual

Include conf/extra/httpd-vhosts.conf

```

>_We can configure HTTP Apache Server to serve up our web pages as though they were actually located on `http://www.example.com` instead of `localhost/example.com` (if `example.com` was project directory in htdocs default document root folder.)_

In order to connect to a `server`, the `client` will first have to resolve the `servername` to an `IP address` - the location on the Internet where the server resides. Thus, in order for your web server to be reachable, it is necessary that the `servername` be in `DNS`. If we are testing a server that is not Internet-accessible, we can put host names in our `hosts` file in order to do local resolution.

This is the two steps process. _First_, we have to _redirect_ website or application _url_ to our computer. _Secondly_, we then get the Apache to *`redirect`* the _url_ to our source code _directory_.

+ [Re-directing _url_ to the IP adress (Host computer)](#firstly_redirecting_url)
+ [Apache *re-direct* the url to source code directory](#secondly_redirecting_url)

<a name="firstly_redirecting_url"></a>
#### Re - directing _url_ to the host computer

When we _type_ and _search_ the website / application *`name/address/url`* into the web browser URL field _(Let’s say we type: http://example.com)_, the _web browser_ sends the provided _url_ to a _Domain Name Server_ that looks-up for text named _http://example.com_ and returns the _Internet Protocol (IP)_ address for it. Then, web browser connects to the _IP address_ it finds.

Before our web browser *`queries`* _DNS_. *Firstly* checks a file called *`hosts`* on our computer. If the requested *`url`* is found, then the web browser uses the *`IP address`* found in the file. To re-direct _url_ to the local host computer:

##### Let's Locate our `hosts` file.

Since we are testing a server that is not Internet-accessible, we can put host names in our `hosts` file in order to do `local resolution`. For example, we might want to put a record in our hosts file to map a request for [www.example.com]() or [example.com]() to our local system, for testing purposes. A hosts file will probably be located at:

```sh

c:\windows\system32\drivers\etc

```

##### Editing host file

*Open* the file with _text editor_ like Notepad++, Notepad, Atom. _Don’t use word processor because it can mess up the file when you save it_. To edit this file you will need administrative rights.

`Domain name(s):` Add domain name(s) and IP address to *redirect* to locally on our computer. Each line must begin with the IP address: *127.0.0.1*, then follow the *name of the domain* which we wish to _redirect_ to the _IP address_. _We can only enter one name per line_. Addresses names like _http://example.com and http://www.example.com_ are different and each need a separate line.

```sh

# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost

127.0.0.1 example.com
127.0.0.1 www.example.com

```

<a name="secondly_redirecting_url"></a>
#### Apache *re-edirect* the url to source code directory

*Then open* `c:\devtools\apache\conf\extra\httpd-vhosts.conf` file and Add the source code or file configuration below at the bottom of the file.

```sh

<VirtualHost *:80>
  ServerAdmin webmaster@example.com
  DocumentRoot "${SRVROOT}/htdocs/example.com"
  ServerName example.com
  ServerAlias www.example.com

  <Directory "C:/DevTools/Apache24/htdocs/example.com">
    Require all granted
    Options Indexes
    AllowOverride All
  </Directory>

  UnsetEnv PHPRC

  <FilesMatch "\.php$">
    SetHandler application/x-httpd-php5.6.9-cgi
  </FilesMatch>

  ErrorLog "logs/example.com-error.log"
  CustomLog "logs/example.com-access.log" common
</VirtualHost>

```

|                    | Description                                          |
| ------------------ | ---------------------------------------------------- |
| *DocumentRoot* | *Add the path where our website / application source code is located. We can use an absolute path / relative path.*              |
| *ServerName*   | *Add the name of the domain*                         |
| *ServerAlias*  | *We can add as many alternate domains (separated by spaces) as you like that are supposed to resolve to this virtual host.*     |

  *Remember that http://www.example.com is different from http://example.com, but we want them to be on the same web server.*

  _Save the file and restart the Apache Server for the changes to take effect. We could also just reboot the computer, but that is overkill_.

  *Open command prompt to stop, test and start Apache to see if changes are successful:*

  ```sh
  # To stop apache server press CTRL + c
  httpd -t
  httpd -w

  ```

*Now,* go to `C:\devtools\apache\htdocs` and create project directory called `example.com` and file called `index.php`, then add `phpinfo()` build-in function on index.php file as shown below:

```sh

mkdir example.com
cd example.com
touch index.php

```

###### Add phpinfo() build in function on index.php

```sh

<?php

 echo phpinfo();

```

+ [http://localhost](http://localhost:8080/)  - PHP build TS 8.4.3
+ [http://localhost/example.com](http://localhost/test) - PHP build TS 5.6.9

![phpinfo() 5.6.9 version](../assets/569.jpg "HTTP Apache lounge is successful installed.")

*` Last modified: 2024-02-04 time: 18:28PM `*
