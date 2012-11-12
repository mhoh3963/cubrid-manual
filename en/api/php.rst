************
PHP 드라이버
************


**PHP Driver**

**PHP Overview**

CUBRID PHP driver implements an interface to enable access from application in PHP to CUBRID database. Every function offered by CUBRID PHP driver has a prefix
**cubrid_**
such as cubrid_connect() and cubrid_connect_with_url().

The official one is available as a PECL package. PECL is a repository for PHP extensions, providing a directory of all known extensions and holding facilities for downloading and development of PHP extensions. For more information about PECL, visit
`http://pecl.php.net/ <http://pecl.php.net/>`_
.

CUBRID PHP driver is written based on CCI API so affected by CCI configurations such as
**CCI_DEFAULT_AUTOCOMMIT**
.

To download PHP driver or get the latest information, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver>`_
.

**Installing and Configuring PHP**

The easiest and fastest way to get all applications installed on your system is to install CUBRID, Apache web server, PHP, and CUBRID PHP driver at the same time. For details, see
`http://www.cubrid.org/wiki_apis/entry/install-cubrid-with-apache-and-php-on-ubuntu <http://www.cubrid.org/wiki_apis/entry/install-cubrid-with-apache-and-php-on-ubuntu>`_
.

**For Linux**

**Configuring the Environment**

*   CUBRID: 2008 R3.0 (8.3.0) or later



*   Operating system: 32-bit or 64-bit Linux



*   Web server: Apache



*   PHP: 5.2 or 5.3 (
    `http://php.net/downloads.php <http://php.net/downloads.php>`_
    )



**Installing CUBRID PHP Driver using PECL**

If
**PECL**
package has been installed on your system, the installation of CUBRID PHP driver is straightforward.
**PECL**
will download and compile the driver for you. If you do not have
**PECL**
installed, follow the instructions at
`http://www.cubrid.org/wiki_apis/entry/installing-cubrid-php-driver-using-pecl <http://www.cubrid.org/wiki_apis/entry/installing-cubrid-php-driver-using-pecl>`_
to get it installed.

*   Enter the following command to install the latest version of CUBRID PHP driver.



sudo pecl install cubrid

If you need earlier versions of the driver, you can install exact versions as follows:

sudo pecl install cubrid-8.3.0.0005

During the installation, you will be prompted to enter
**CUBRID base install dir autodetect :**
. Just to make sure your installation goes smoothly, enter the full path to the directory where you have installed CUBRID. For example, if CUBRID has been installed at
**/home/cubridtest/CUBRID**
, then enter
**/home/cubridtest/CUBRID**
.

*   Edit the configuration file.



*   If you are using CentOS 6.0 and later or Fedora 15 and later, create a file named
    **pdo_cubrid.ini**
    , enter a command line 
    **extension=pdo_cubrid.so**
    , and store the fine in the
    **/etc/php.d**
    directory.



*   If you are using earlier versions of CentOS 6.0 or Fedora 15, edit the
    **php.ini**
    file (default location:
    **/etc/php5/apache2/**
    or
    **/etc/**
    ) and add the following two command lines at the end of the file.



[CUBRID]

extension=cubrid.so

*   Restart the web server to apply changes.



**Installing using apt-get on Ubuntu**

*   If you do not have PHP itself installed, install it using the following command; if you have PHP installed on your system, skip this step.



sudo apt-get install php5

*   To install CUBRID PHP driver using
    **apt-get**
    , we need to add CUBRID's repository so that Ubuntu knows where to download the packages from and tell the operating system to update its indexes.



sudo add-apt-repository ppa:cubrid/cubrid

sudo apt-get update

*   Now install the driver.



sudo apt-get install php5-cubrid

To install earlier versions, indicate the version as:

sudo apt-get install php5-cubrid-8.3.1

This will copy the
**cubrid.so**
driver to
**/usr/lib/php5/2009***
and add the following configuration lines to
**/etc/php5/apache2/php.ini**
.

[PHP_CUBRID]

extension=cubrid.so

*   Restart the web server so that PHP can read the module.



service apache2 restart

**Installing using Yum on Fedora/CentOS**

*   To install CUBRID PHP driver using
    **yum**
    command, we need to tell
    **Yum**
    where to look for CUBRID package. First, visit one of the following links depending on your operating system.



*   CentOS:
    `http://www.cubrid.org/?mid=yum_repository&os=centos <http://www.cubrid.org/?mid=yum_repository&os=centos>`_



*   Fedora:
    `http://www.cubrid.org/?mid=yum_repository&os=fedora <http://www.cubrid.org/?mid=yum_repository&os=fedora>`_



*   Choose CUBRID version. You will be given a list of links for your particular version. For example, the following link is provided for Fedora 16 where fc16 means this operating system version.



rpm -i http://yumrepository.cubrid.org/cubrid_repo_settings/9.0.0/cubridrepo-9.0.0-1.fc16.noarch.rpm

For CentOS, el6.2 means CentOS version 6.2.

rpm -i http://yumrepository.cubrid.org/cubrid_repo_settings/9.0.0/cubridrepo-9.0.0-1.el6.2.noarch.rpm

Executing this command will tell
**Yum**
where to look for CUBRID package.

*   Execute the command below to install CUBRID PHP driver.



yum install php-cubrid

*   Restart the web server.



service httpd restart

**For Windows**

**Requirements**

*   CUBRID: 2008 R3.0 (8.3.0) or later



*   Operating system: 32-bit or 64 bit Windows



*   Web server: Apache or IIS



*   PHP: 5.2 or 5.3 (
    `http://windows.php.net/download/ <http://windows.php.net/download/>`_
    )



**Using CUBRID PHP Driver Installer**

The CUBRID PHP API Installer is a Windows installer which automatically detects the CUBRID and PHP version and installs the proper driver for you by copying it to the default PHP extensions directory and adding the extension load directives to the
**php.ini**
file. In this section, we will explain how to use the CUBRID PHP API Installer to install the CUBRID PHP extension on Windows.

In case you want to remove the CUBRID PHP driver, you just have to run the CUBRID PHP API Installer again in uninstall mode (like any other un-installer on Windows) and it will reset all the changes made during installation.

Before you install CUBRID PHP driver, make sure that paths of PHP and CUBRID are added in the system variable,
**Path**
.

*   Download the CUBRID PHP API installer for Windows from the link below. The current installer includes the drivers for all CUBRID versions.



`http://www.cubrid.org/?mid=downloads&item=php_driver&os=windows <http://www.cubrid.org/?mid=downloads&item=php_driver&os=windows>`_

*   To install the PHP extension, run the installer. Once the installer starts, click the [Next] button.



*   Agree with the BSD license terms and click the [Next] button.



*   Choose where you would like to install this CUBRID PHP API Installer and click the [Next] button. You should choose a new folder for this installer like like
    **C:\Program Files\CUBRID PHP API**
    .



*   Give a folder name and click the [Install] button. If you fail installation, you should probably receive an error message. In this case, see
    `Add Environment Variables to System PATH <#api_api_php_install_htm_error_me_8941>`_
    below.



*   If no error message is displayed, this should install the CUBRID PHP extension and update your
    **php.ini**
    file. Click [Finish] to close the installer.



*   For changes to take place, restart your web server and execute the phpinfo() to confirm CUBRID has successfully been installed.



|image56_png|

**Configuring the environment**

If you have received an error messages, follow the steps below; if you can see CUBRID in phpinfo(), you do not need to look further. By default, when you install CUBRID, it automatically adds its installation directory to the
**Path**
system environment variable. To verify the variable have been correctly configured, launch the command prompt ([Start] > [Programs] > [Accessories] > [Command Prompt]) and enter the following commands one by one.

*   Enter command below in the command prompt as follows.



php --version

You can see the PHP version like below if it is properly configured.

C:\Users\Administrator>php --version

PHP 5.2.9 <cli> <built: Feb 25 2009 15:52:24>

*   Enter command as follows.  



php --version

You can see the CUBIRD version like below if it is properly configured.

C:\Users\Administrator>cubrid --version

cubrid <cubrid utilities> R2.1

If you can not get the result like above, it is highly likely that your PHP and CUBRID installations went wrong. Try to reinstall them and recheck again. If the path is not automatically specified even after you complete reinstallation, you can do it manually.

*   Right-click [My Computer] and select [Properties]. The [System Properties] dialog box will appear.



*   Go to [Advanced] tab and click on [Environment Variables].



*   Select the variable called
    **Path**
    in the [System variables] box and click [Edit] button. You will notice that the value of that variable contains system paths separated by semi-colon.



*   Add the paths for CUBRID and PHP in that variable. For example, if PHP is installed in
    **C:\Program Files\PHP**
    and also CUBRID in
    **C:\CUBRID\bin**
    , you will have to append (do not overwrite, just append) these values to the path like
    **C:\CUBRID\bin;C:\Program Files\PHP**
    .



*   Click [OK] to save and close the dialog box.



*   To confirm you have done everything correct, check the variable presence in the command prompt.



**Downloading and Installing Compiled CUBRID PHP Driver**

First, download CUBRID PHP/PDO driver of which versions match the versions of your operating system and PHP installed from  
`http://www.cubrid.org/?mid=downloads&item=php_driver&os=windows&php=detect&driver=detect <http://www.cubrid.org/?mid=downloads&item=php_driver&os=windows&php=detect&driver=detect>`_
.

After you download the driver, you will see the 
**php_cubrid.dll**
file for CUBRID PHP driver or the 
**php_pdo_cubrid.dll**
file for CUBRID PDO driver. Follow the steps below to install it.

*   Copy this driver to the default PHP extensions directory (usually located at
    **C:\Program Files\PHP\ext**
    ).



*   Set your system environment. Check if the environment variable
    **PHPRC**
    is
    **C:\Program Files\PHP**
    and system variable path is added with
    **%PHPRC%**
    and
    **%PHPRC\ext**
    .



*   Edit
    **php.ini**
    (
    **C:\Program Files\PHP\php.ini**
    ) and add the following two command lines at the end of the
    **php.ini**
    file.



[PHP_CUBRID]

extension=php_cubrid.dll

For CUBRID PDO driver, add command lines below.

[PHP_PDO_CUBRID]

extension = php_pdo_cubrid.dll

*   Restart your web server to apply changes.



**Note**
To get the latest information information about PHP driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver>`_
.

**Building CUBRID PHP Driver from Source Code**

**For Linux**

In this section, we will introduce the way of building CUBRID PHP driver for Linux.

**Configuring the environment**

*   CUBRID:
    Install CUBRID.
    Make sure the environment variable
    **%CUBRID%**
    is defined in your system.



*   PHP 5.3 source code: You can download PHP source code from
    `http://php.net/downloads.php <http://php.net/downloads.php>`_
    .



*   Apache 2: It can be used to test PHP.



*   CUBRID PHP driver source code: You can download the source code from
    `http://www.cubrid.org/?mid=downloads&item=php_driver <http://www.cubrid.org/?mid=downloads&item=php_driver>`_
    . Make sure that the version you download is the same as the version of CUBRID which has been installed on your system.



**Compiling CUBRID PHP driver**

*   Download the CUBRID PHP driver, extract it, and enter the directory.



$> tar zxvf php-<version>.tar.gz (or tar jxvf php-<version>.tar.bz2)

$> cd php-<version>/ext 

*   Run
    **phpize**
    . For more information about getting
    **phpize**
    , see
    `Remark <#api_api_php_build_htm_remark>`_
    .



cubrid-php> /usr/bin/phpize

*   Configure the project. It is recommended to execute
    **./configure –h**
    so that you can check the configuration options (we assume that Apache 2 has been installed in
    **/usr/local**
    ).



cubrid-php>./configure --with-cubrid --with-php-config=/usr/local/bin/php-config

*   --with-cubrid=shared: Includes CUBRID support.



*   --with-php-config=PATH: Enters an absolute path of php-config including the file name.



*   Build the project. If it is successfully compiled, the
    **cubrid.so**
    file will be created in the
    **/modules**
    directory.



*   Copy the
    **cubrid.so**
    to the
    **/usr/local/php/lib/php/extensions**
    directory; the
    **/usr/local/php**
    is a PHP root directory.



cubrid-php> mkdir /usr/local/php/lib/php/extensions

cubrid-php> cp modules/cubrid.so /usr/local/php/lib/php/extensions

*   In the
    **php.ini**
    file, set the
    **extension_dir**
    variable and add the CUBRID PHP driver to the
    **extension**
    variable as shown below.



extension_dir = "/usr/local/php/lib/php/extension/no-debug-zts-xxx"

extension = cubrid.so

**Testing CUBIRD PHP driver installation**

*   Create a
    **test.php**
    file as follows:



<?php phpinfo(); ?>

*   Use web browser to visit
    http://localhost/test.php. If you can see the following result, it means that installation is successfully completed.



+------------+------------+
| **CUBRID** | **Value**  |
|            |            |
+------------+------------+
| Version    | 9.0.0.XXXX |
|            |            |
+------------+------------+

**Remark**

What is
**phpize**
? Where can I get it?

**phpize**
is a shell script to prepare the PHP extension for compiling. You can get it when you install PHP because it is automatically installed with PHP installation, in general. If it you do not have
**phpize**
installed on your system, you can get it by following the steps below.

*   Download the PHP source code. Make sure that the PHP version works with the PHP extension that you want to use. Extract PHP source code and enter its root directory.



$> tar zxvf php-<version>.tar.gz (or tar jxvf php-<version>.tar.bz2)

$> cd php-<version>

*   Configure the project, build, and install it. You can specify the directory you want install PHP by using the option,
    **--prefix**
    .



php-root> ./configure --prefix=prefix_dir; make; make install

*   You can find
    **phpize**
    in the
    **prefix_dir/bin**
    directory.



**For Windows**

In this section, we will introduce three ways of building CUBRID PHP driver for Windows.

If you have no idea of which version you choose, read the following contents first.

*   If you are using PHP with Apache 1 or Apache 2, you should use the VC6 versions of PHP.



*   If you are using PHP with IIS, you should use the VC9 versions of PHP.



VC6 versions are compiled with the legacy Visual Studio 6 compiler; VC9 versions are compiled with the Visual Studio 2008 compiler. The VC9 versions have more improvements in performance and stability.

The VC9 versions require you to have the Microsoft 2008 C++ Runtime (x86) or the Microsoft 2008 C++ Runtime (x64) installed. Do not use VC9 versions with binaries provided by the Apache Software Foundation (
`http://www.apache.org/ <http://www.apache.org/>`_
).

**Building CUBRID PHP Driver with VC9 for PHP 5.3**

**Configuring the environment**

*   CUBRID:
    Install CUBRID.
    Make sure the environment variable
    **%CUBRID%**
    is defined in your system.



*   Visual Studio 2008: You can alternately use the free Visual C++ Express Edition or the Visual C++ 9 compiler included in the Windows SDK v6.1 if you are familiar with a makefile. Make sure that you have the Microsoft Visual C++ Redistributable Package installed on your system to use CUBRID PHP VC9 driver.



*   PHP 5.3 binaries: You can install VC9 x86 Non Thread Safe or VC9 x86 Thread Safe. Make sure that the
    **%PHPRC%**
    system environment variable is correctly set.



In the [Property Pages] dialog box, select [General] under the [Linker] tree node. You can see
**$(PHPRC)**
in [Additional Library Directories].

|image57_jpg|

*   PHP 5.3 source code: Remember to get the source code that matches your binary version. After you extract the PHP 5.3 source code, add the
    **%PHP5_SRC%**
    system environment variable and set its value to the path of PHP 5.3 source code.



In the [Property Pages] dialog box, select [General] under the [C/C++] tree node. You can see
**$(PHP5_SRC)**
in [Additional Include Directories].

|image58_jpg|

*   CUBRID PHP driver source code: You can download CUBRID PHP driver source code of which version is the same as the version of CUBRID that have been installed on your system. You can get it from
    `http://www.cubrid.org/?mid=downloads&item=php_driver <http://www.cubrid.org/?mid=downloads&item=php_driver>`_
    .



**Note**
You do not need to build PHP 5.3 from source code but configuring a project is required. If you do not make configuration settings, you will get the message that a header file (
**config.w32.h**
) cannot be found.
Read
`http://wiki.php.net/internals/windows/stepbystepbuild <http://wiki.php.net/internals/windows/stepbystepbuild>`_
to get more detailed information.

**Building CUBRID PHP driver with VC9 for PHP 5.3**

*   Open the
    **php_cubrid.vcproj**
    file under the
    **\win**
    directory. In the [Solution Explorer] pane, right-click on the
    **php_cubrid**
    (project name) and select [Properties].



|image59_jpg|

*   In the [Property Page] dialog box, click the [Configuration Manager] button. Select one of four values among Release_TS, Release_NTS, Debug_TS, and Debug_NTS in [Configuration] of [Project contexts] and click the [Close] button.



|image60_jpg|

*   After you complete the properties modification, click the [OK] button and press the <F7> key to compile the driver. Then, we have the
    **php_cubrid.dll**
    file built.



*   You need to make PHP recognize the
    **php_cubrid.dll**
    file as an extension. To do this:



*   Create a new folder named
    **cubrid**
    where PHP has been installed and copy the 
    **php_cubrid.dll**
    file to the
    **cubrid**
    folder. You can also put the
    **php_cubrid.dll**
    file in
    **%PHPRC%\ext**
    if this directory exists.



*   In the php.ini file, enter the path of the
    **php_cubrid.dll**
    file as an extension_dir variable value and enter
    **php_cubrid.dll**
    as an extension value.



**Building CUBRID PHP Driver with VC6 for PHP 5.2/5.3**

**Configuring the environment**

*   CUBRID:
    Install CUBRID.
    Make sure that the environment variable
    **%CUBRID%**
    is defined in your system.



*   Visual C++ 6.0 SP6



*   Windows Server Feb. 2003 SDK: It is recommended to use Windows Server Feb. 2008 SDK because every official release and snapshot are compiled with Visual C++ 6.0 SP6 and Windows Server Feb. 2003 SDK.



You can configure the default settings without using this SDK; however, there is possibility that an error would occur while building the driver. In this case, you should fix the error yourself.

*   PHP 5.3/5.2 binaries: You can install VC6 x86 Non Thread Safe or VC6 x86 Thread Safe. Make sure that the value of the
    **%PHPRC%**
    system environment variable is correctly set.



In the [Project Settings] dialog box, you can find
**$(PHPRC)**
in [Additional library path] of the [Link] tab.

|image61_jpg|

*   PHP 5.2/5.3 source code: Remember to get the source that matches your binary version. After you extract the PHP 5.3 source code, add the
    **%PHP5_SRC%**
    system environment variable and set its value to the path of PHP 5.3 source code.



In the [Project Settings] dialog box of VC6 project, you can find
**$(PHP5_SRC)**
in [Additional include directories] of the [C/C++] tab.

|image62_jpg|

*   CUBRID PHP driver source code: You can download CUBRID PHP driver source code of which version is the same as the version of CUBRID that has been installed on your system. You can get it from
    `http://www.cubrid.org/?mid=downloads&item=php_driver <http://www.cubrid.org/?mid=downloads&item=php_driver>`_
    .



**Note**
If you build CUBRID PHP driver with PHP 5.3 source code, you need to make some configuration settings for PHP 5.3 on Windows. If you do not make these settings, you will get the message that a header file (
**config.w32.h**
) cannot be found. Read
`http://wiki.php.net/internals/windows/stepbystepbuild <http://wiki.php.net/internals/windows/stepbystepbuild>`_
to get more detailed information.

**Building CUBRID PHP driver**

*   Open the project in the [Build] menu and then select [Set Active Configuration].



|image63_jpg|

*   There are four types of configuration settings (Win32 Release_TS, Win32 Release, Win32 Debug_TS, and Win32 Debug). Select one of them depending on your system and then click the [OK] button.



|image64_jpg|

*   After you complete the properties modification, click the [OK] button and press the <F7> key to compile the driver. Then you have the
    **php_cubrid.dll**
    file built.



*   You need to make PHP recognize the
    **php_cubrid.dll**
    file as an extension. To do this:



*   Create a new folder named
    **cubrid**
    where PHP is installed and copy
    **php_cubrid.dll**
    to the
    **cubrid**
    folder. You can also put
    **php_cubrid.dll**
    in
    **%PHPRC%\ext**
    if this directory exists.



*   Set the
    **extension_dir**
    variable and add CUBRID PHP driver to
    **extension**
    variable in the
    **php.ini**
    file.



**Building CUBRID PHP Driver for 64-bit Windows**

**PHP for 64-bit Windows**

We do not provide 64-bit Windows CUBRID PHP driver, mainly because there is no official 64-bit Windows PHP at windows.php.net (only x86 versions are available). But sometimes you need 64-bit Windows binaries for PHP. In that case you can build it from source codes. Best of all, some guys have already done this (see
`http://www.anindya.com/ <http://www.anindya.com/>`_
). Here, we will not describe how to build x64 PHP itself.

You can find the supported compilers to build PHP on Windows at
`https://wiki.php.net/internals/windows/compiler <https://wiki.php.net/internals/windows/compiler>`_
. You can see that both VC++ 8 (2005) and VC++ 9 (2008 SP1 only) can be used to build 64-bit PHP. Earlier versions of Visual C++ 2005, the Windows Server Fed. 2003 SDK was the only way to build 64-bit Windows applications.

**Apache for 64-bit Windows**

There is no official Apache for 64-bit Windows either. Instead, you can use IIS as your Windows Web Server on 64-bit Windows. If you really need VC9 x64 versions of Apache, you can find it at
`http://www.anindya.com/ <http://www.anindya.com/>`_
.

**Configuring the environment**

*   CUBRID for 64-bit Windows: You can install the latest version of CUBRID for 64-bit Windows. Make sure the environment variable
    **%CUBRID%**
    is defined in your system.



*   Visual Studio 2008: You can alternately use the free Visual C++ Express Edition or the Visual C++ 9 compiler in the Windows SDK v6.1 if you are familiar with a makefile.



*   SDK 6.1: If you are using VC9, you need Microsoft Windows SDK for Windows Server 2008 and .NET Framework 3.5 (also known as the SDK 6.1).



*   PHP 5.3 binaries for 64-bit Windows: You can build your own VC9 x64 PHP with SDK 6.1 or you can get it at
    `http://www.anindya.com/ <http://www.anindya.com>`_
    . Both VC9 x64 Non Thread Safe and VC9 x64 Thread Safe are available. After you have installed it, check if the value of system environment variable
    **%PHPRC%**
    is correctly set.



*   PHP 5.3 source code: Remember to get the src package that matches your binary version. After you extract the PHP 5.3 src, add system environment variable
    **%PHP5_SRC%**
    and set its value to the path of PHP 5.3 source code. In the VC9 [Property Pages] dialog box, select [General] under the [C/C++] tree node. You can see
    **$(PHP5_SRC)**
    in [Additional Include Directories].



*   CUBRID PHP driver source code: You can download CUBRID PHP driver source code of which version is the same as the version of CUBRID that is installed on your system. You can get it from
    `http://www.cubrid.org/?mid=downloads&item=php_driver <http://www.cubrid.org/?mid=downloads&item=php_driver>`_
    .



**Note**
You do not need to build PHP 5.3 from source code; however, configuring a project is required. If you do not make configuration settings, you will get the message that a header file (
**config.w32.h**
) cannot be found.
Read
`https://wiki.php.net/internals/windows/stepbystepbuild <https://wiki.php.net/internals/windows/stepbystepbuild>`_
to get more detailed information.

**Configuring PHP 5.3**

*   After you have installed SDK 6.1, click the [CMD Shell] shortcut under the [Microsoft Windows SDK v6.1] folder (Windows Start menu).



|image65_png|

*   Run
    **setenv /x64 /release**
    .



|image66_png|

*   Enter PHP 5.3 source code directory in the command prompt and run
    **buildconf**
    to generate the
    **configure.js**
    file.



|image67_png|

Or you can also double-click the
**buildconf.bat**
file.

|image68_png|

*   Run the 
    **configure**
    command to configure the PHP project.



|image69_png|

|image70_png|

**Building CUBRID PHP dirver**

*   Open the
    **php_cubrid.vcproj**
    file under the
    **\win**
    directory. In the [Solution Explorer] on the left, right-click on the
    **php_cubrid**
    project name and select [Properties].



*   On the top right corner of the [Property Pages] dialog box, click [Configuration Manager].



|image71_png|

*   In the [Configuration Manager] dialog box, you can see four types of configurations (Release_TS, Release_NTS, Debug_TS, and Debug_NTS) in the [Active solution configuration] dropdown list. Select
    **New**
    in the dropdown list so that you can create a new one for your x64 build.



|image72_png|

*   In the [New Solution Configuration] dialog box, enter a value in the
    **Name**
    box (e.g.,
    **Release_TS_x64**
    ). In the [Copy settings from] dropdown list, select the corresponding x86 configuration and click [OK].



|image73_png|

*   In the [Configuration Manager] dialog box, select the value
    **x64**
    in the [Platform] dropdown list. If it does not exist, select
    **New**
    .



|image74_png|

*   In the [New Project Platform] dialog box, select
    **x64**
    option in the [New platform] dropdown list.



|image75_png|

*   Click [OK] and close the [Configuration Manager].



*   In the [Property Pages] dialog box, select [Preprocessor] under the [C/C++] tree node. In [Preprocessor Definitions], delete
    **_USE_32BIT_TIME_T**
    and click [OK] to close the dialog box.



|image76_png|

*   Press the <F7> key to compile. Now you will get the CUBRID PHP driver for 64-bit Windows.



**Note**
To get the latest information about PHP driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver>`_
.

**PHP Programming**

**General Features**

**Connecting to a Database**

The first step of database applications is to use
`cubrid_connect <http://www.php.net/manual/en/function.cubrid-connect.php>`_
() or
`cubrid_connect_with_url <http://www.php.net/manual/en/function.cubrid-connect-with-url.php>`_
() function which provides database connection. Once
`cubrid_connect <http://www.php.net/manual/en/function.cubrid-connect.php>`_
() or 
`cubrid_connect_with_url <http://www.php.net/manual/en/function.cubrid-connect-with-url.php>`_
() function is executed successfully, you can use any functions available in the database. It is very important to call the
`cubrid_disconnect <http://www.php.net/manual/en/function.cubrid-disconnect.php>`_
() function before applications are  terminated. The
`cubrid_disconnect <http://www.php.net/manual/en/function.cubrid-disconnect.php>`_
() function terminates the current transaction as well as the connection handle and all request handles created by the
`cubrid_connect <http://www.php.net/manual/en/function.cubrid-connect.php>`_
() function.

**Warning**
The database connection in thread-based programming must be used independently each other..

**Transactions and Auto-Commit**

CUBRID PHP supports transaction and auto-commit mode. Auto-commit mode means that every query that you run has its own implicit transaction. You can use the
`cubrid_get_autocommit <http://www.php.net/manual/en/function.cubrid-get-autocommit.php>`_
() function to get the status of current connection auto-commit mode and use the
`cubrid_set_autocommit <http://www.php.net/manual/en/function.cubrid-set-autocommit.php>`_
() function to enable/disable auto-commit mode of current connection. In auto-commit mode, any transactions being executed are committed regardless of whether it is set to
**ON**
or
**OFF**
.

The default value of auto-commit mode upon application startup is configured by the
**CCI_DEFAULT_AUTOCOMMIT**
(broker parameter). If the broker parameter value is not configured, the default value is set to
**ON**
. You can also use the
`cubrid_connect_with_url <http://www.php.net/manual/en/function.cubrid-connect-with-url.php>`_
() function to set auto-commit mode as example shown below.

$con = cubrid_connect_with_url("cci:CUBRID:localhost:33000:demodb:dba::?autocommit=true");

If you set auto-commit mode to
**OFF**
in the
`cubrid_set_autocommit <http://www.php.net/manual/en/function.cubrid-set-autocommit.php>`_
() function, you can handle transactions by specifying a proper function; to commit transactions, use the
`cubrid_commit <http://www.php.net/manual/en/function.cubrid-commit.php>`_
() function and to roll back transactions, use the
`cubrid_rollback <http://www.php.net/manual/en/function.cubrid-rollback.php>`_
() function. If you use the
`cubrid_disconnect <http://www.php.net/manual/en/function.cubrid-disconnect.php>`_
() function, transactions will be disconnected and jobs which have not been committed will be rolled back.

**Processing Queries**

**Executing queries**

Followings are the basic steps to execute queries.

*   Creating a connection handle



*   Creating a request handle for an SQL query request



*   Fetching result



*   Disconnecting the request handle



$con = cubrid_connect("192.168.0.10", 33000, "demodb");

if($con) {

    $req = cubrid_execute($con, "select * from code");

    if($req) {

        while ($row = cubrid_fetch($req)) {

            echo $row["s_name"];

            echo $row["f_name"];

        }

        cubrid_close_request($req);

    }

    cubrid_disconnect($con);

}

**Column types and names of the query result**

The
`cubrid_column_types <http://www.php.net/manual/en/function.cubrid-column-types.php>`_
() function is used to get arrays containing column types and the
`cubrid_column_types <http://www.php.net/manual/en/function.cubrid-column-types.php>`_
() functions is used to get arrays containing colunm names.

$req = cubrid_execute($con, "select host_year, host_city from olympic");

if($req) {

    $col_types = cubrid_column_types($req);

    $col_names = cubrid_column_names($req);

 

    while (list($key, $col_type) = each($col_types)) {

        echo $col_type;

    }

    while (list($key, $col_name) = each($col_names))

        echo $col_name;

    }

    cubrid_close_request($req);

}

**Controlling a cursor**

The
`cubrid_move_cursor <http://www.php.net/manual/en/function.cubrid-move-cursor.php>`_
() function is used to move a cursor to a specified position from one of three points: beginning of the query result, current cursor position, or end of the query result).

$req = cubrid_execute($con, "select host_year, host_city from olympic order by host_year");

if($req) {

    cubrid_move_cursor($req, 20, CUBRID_CURSOR_CURRENT)

    while ($row = cubrid_fetch($req, CUBRID_ASSOC)) {

        echo $row["host_year"].” “;

        echo $row["host_city"].”\n”;

    }

}

**Result array types**

One of the following three types of arrays is used in the result of the
`cubrid_fetch <http://www.php.net/manual/en/function.cubrid-fetch.php>`_
() function. The array types can be determined when the
`cubrid_fetch <http://www.php.net/manual/en/function.cubrid-fetch.php>`_
() function is called. Of array types, the associative array uses string indexes and the numeric array uses number indexes. The last array includes both associative and numeric arrays.

*   Numeric array



while (list($id, $name) = cubrid_fetch($req, CUBRID_NUM)) {

    echo $id;

    echo $name;

}

*   Associative array



while ($row = cubrid_fetch($req, CUBRID_ASSOC)) {

    echo $row["id"];

    echo $row["name"];

}

**Catalog Operations**

The
`cubrid_schema <http://www.php.net/manual/en/function.cubrid-schema.php>`_
() function is used to get database schema information such as classes, virtual classes, attributes, methods, triggers, and constraints. The return value of the
`cubrid_schema <http://www.php.net/manual/en/function.cubrid-schema.php>`_
() function is a two-dimensional array.

$pk = cubrid_schema($con, CUBRID_SCH_PRIMARY_KEY, "game");

if ($pk) {

    print_r($pk);

}

 

$fk = cubrid_schema($con, CUBRID_SCH_IMPORTED_KEYS, "game");

if ($fk) {

    print_r($fk);

}

**Error Handling**

When an error occurs, most of PHP interfaces display error messages and return false or -1. The
`cubrid_error_msg <http://www.php.net/manual/en/function.cubrid-error-msg.php>`_
(),
`cubrid_error_code <http://www.php.net/manual/en/function.cubrid-error-code.php>`_
() and
`cubrid_error_code_facility <http://www.php.net/manual/en/function.cubrid-error-code-facility.php>`_
() functions are used to check error messages, error codes, and error facility codes.

The return value of the
`cubrid_error_code_facility <http://www.php.net/manual/en/function.cubrid-error-code-facility.php>`_
() function is one of the followings (
**CUBRID_FACILITY_DBMS**
(DBMS error),
**CUBRID_FACILITY_CAS**
(CAS server error),
**CUBRID_FACILITY_CCI**
(CCI error), or
**CUBRID_FACILITY_CLIENT**
(PHP module error).

**CUBRID Characteristics**

**Using OIDs**

The OID value in the currently updated f record by using the
`cubrid_current_oid <http://www.php.net/manual/en/function.cubrid-current-oid.php>`_
function if it is used together with query that can update the
**CUBRID_INCLUDE_OID**
option in the
`cubrid_execute <http://www.php.net/manual/en/function.cubrid-execute.php>`_
() function.

$req = cubrid_execute($con, "select * from person where id = 1", CUBRID_INCLUDE_OID);

if ($req) {

    while ($row = cubrid_fetch($req)) {

        echo cubrid_current_oid($req);

        echo $row["id"];

        echo $row["name"];

    }

    cubrid_close_request($req);

}

Values in every attribute, specified attributes, or a single attribute of an instance can be obtained by using OIDs. If any attributes are not specified in the
`cubrid_get <http://www.php.net/manual/en/function.cubrid-get.php>`_
() function, values in every attribute are returned (a). If attributes is specified in the array data type, the array containing the specified attribute value is returned in the associative array (b). If a single attribute it is specified in the string type, a value of the attributed is returned (c).

$attrarray = cubrid_get ($con, $oid); // (a)

$attrarray = cubrid_get ($con, $oid, array("id", "name")); // (b)

$attrarray = cubrid_get ($con, $oid, "id"); // (c)

The attribute values of an instance can be updated by using OIDs. To update a single attribute value, specify attribute name and value in the string type (a). To update multiple attribute values, specify attribute names and values in the associative array (b).

$cubrid_put ($con, $oid, "id", 1); // (a)

$cubrid_put ($con, $oid, array("id"=>1, "name"=>"Tomas")); // (b)

**Using Collections**

You can use the collection data types through PHP array data types or functions that support array data types. The following example shows how to fetch query result by using the
`cubrid_fetch <http://www.php.net/manual/en/function.cubrid-fetch.php>`_
() function.

$row = cubrid_fetch ($req);

$col = $row["customer"];

while (list ($key, $cust) = each ($col)) {

   echo $cust;

}

You can get values of collection attributes. The example shows how to get values of collection attributes by using the
`cubrid_col_get <http://www.php.net/manual/en/function.cubrid-col-get.php>`_
() function.

$tels = cubrid_col_get ($con, $oid, "tels");

while (list ($key, $tel) = each ($tels)) {

   echo $tel."\n";

}

You can directly update values of collection types by using cubrid_set_add() or cubrid_set_drop() function.

$tels = cubrid_col_get ($con, $oid, "tels");

while (list ($key, $tel) = each ($tels)) {

   $res = cubrid_set_drop ($con, $oid, "tel", $tel);

}

cubrid_commit ($con);

**Note**
If a string longer than defined max length is inserted (
**INSERT**
) or updated (
**UPDATE**
), the string will be truncated.

**Note**
To get the latest information about PHP driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver>`_
.

**PHP API**

For more information about PHP API, see PHP CUBRID Functions document at
`http://www.php.net/manual/en/ref.cubrid.php <http://www.php.net/manual/en/ref.cubrid.php>`_
.

*   `cubrid_bind <http://www.php.net/manual/en/function.cubrid-bind.php>`_



*   `cubrid_close_prepare <http://www.php.net/manual/en/function.cubrid-close-prepare.php>`_



*   `cubrid_close_request <http://www.php.net/manual/en/function.cubrid-close-request.php>`_



*   `cubrid_col_get <http://www.php.net/manual/en/function.cubrid-col-get.php>`_



*   `cubrid_col_size <http://www.php.net/manual/en/function.cubrid-col-size.php>`_



*   `cubrid_column_names <http://www.php.net/manual/en/function.cubrid-column-names.php>`_



*   `cubrid_column_types <http://www.php.net/manual/en/function.cubrid-column-types.php>`_



*   `cubrid_commit <http://www.php.net/manual/en/function.cubrid-commit.php>`_



*   `cubrid_connect_with_url <http://www.php.net/manual/en/function.cubrid-connect-with-url.php>`_



*   `cubrid_connect <http://www.php.net/manual/en/function.cubrid-connect.php>`_



*   `cubrid_current_oid <http://www.php.net/manual/en/function.cubrid-current-oid.php>`_



*   `cubrid_disconnect <http://www.php.net/manual/en/function.cubrid-disconnect.php>`_



*   `cubrid_drop <http://www.php.net/manual/en/function.cubrid-drop.php>`_



*   `cubrid_error_code_facility <http://www.php.net/manual/en/function.cubrid-error-code-facility.php>`_



*   `cubrid_error_code <http://www.php.net/manual/en/function.cubrid-error-code.php>`_



*   `cubrid_error_msg <http://www.php.net/manual/en/function.cubrid-error-msg.php>`_



*   `cubrid_execute <http://www.php.net/manual/en/function.cubrid-execute.php>`_



*   `cubrid_fetch <http://www.php.net/manual/en/function.cubrid-fetch.php>`_



*   `cubrid_free_result <http://www.php.net/manual/en/function.cubrid-free-result.php>`_



*   `cubrid_get_autocommit <http://www.php.net/manual/en/function.cubrid-get-autocommit.php>`_



*   `cubrid_get_charset <http://www.php.net/manual/en/function.cubrid-get-charset.php>`_



*   `cubrid_get_class_name <http://www.php.net/manual/en/function.cubrid-get-class-name.php>`_



*   `cubrid_get_client_info <http://www.php.net/manual/en/function.cubrid-get-client-info.php>`_



*   `cubrid_get_db_parameter <http://www.php.net/manual/en/function.cubrid-get-db-parameter.php>`_



*   `cubrid_get_query_timeout <http://www.php.net/manual/en/function.cubrid-get-query-timeout.php>`_



*   `cubrid_get_server_info <http://www.php.net/manual/en/function.cubrid-get-server-info.php>`_



*   `cubrid_get <http://www.php.net/manual/en/function.cubrid-get.php>`_



*   `cubrid_insert_id <http://www.php.net/manual/en/function.cubrid-insert-id.php>`_



*   `cubrid_is_instance <http://www.php.net/manual/en/function.cubrid-is-instance.php>`_



*   `cubrid_lob_close <http://www.php.net/manual/en/function.cubrid-lob-close.php>`_



*   `cubrid_lob_export <http://www.php.net/manual/en/function.cubrid-lob-export.php>`_



*   `cubrid_lob_get <http://www.php.net/manual/en/function.cubrid-lob-get.php>`_



*   `cubrid_lob_send <http://www.php.net/manual/en/function.cubrid-lob-send.php>`_



*   `cubrid_lob_size <http://www.php.net/manual/en/function.cubrid-lob-size.php>`_



*   `cubrid_lock_read <http://www.php.net/manual/en/function.cubrid-lock-read.php>`_



*   `cubrid_lock_write <http://www.php.net/manual/en/function.cubrid-lock-write.php>`_



*   `cubrid_move_cursor <http://www.php.net/manual/en/function.cubrid-move-cursor.php>`_



*   `cubrid_next_result <http://www.php.net/manual/en/function.cubrid-next-result.php>`_



*   `cubrid_num_cols <http://www.php.net/manual/en/function.cubrid-num-cols.php>`_



*   `cubrid_num_rows <http://www.php.net/manual/en/function.cubrid-num-rows.php>`_



*   `cubrid_pconnect_with_url <http://www.php.net/manual/en/function.cubrid-pconnect-with-url.php>`_



*   `cubrid_pconnect <http://www.php.net/manual/en/function.cubrid-pconnect.php>`_



*   `cubrid_prepare <http://www.php.net/manual/en/function.cubrid-prepare.php>`_



*   `cubrid_put <http://www.php.net/manual/en/function.cubrid-put.php>`_



*   `cubrid_rollback <http://www.php.net/manual/en/function.cubrid-rollback.php>`_



*   `cubrid_schema <http://www.php.net/manual/en/function.cubrid-schema.php>`_



*   `cubrid_seq_drop <http://www.php.net/manual/en/function.cubrid-seq-drop.php>`_



*   `cubrid_seq_insert <http://www.php.net/manual/en/function.cubrid-seq-insert.php>`_



*   `cubrid_seq_put <http://www.php.net/manual/en/function.cubrid-seq-put.php>`_



*   `cubrid_set_add <http://www.php.net/manual/en/function.cubrid-set-add.php>`_



*   `cubrid_set_autocommit <http://www.php.net/manual/en/function.cubrid-set-autocommit.php>`_



*   `cubrid_set_db_parameter <http://www.php.net/manual/en/function.cubrid-set-db-parameter.php>`_



*   `cubrid_set_drop <http://www.php.net/manual/en/function.cubrid-set-drop.php>`_



*   `cubrid_set_query_timeout <http://www.php.net/manual/en/function.cubrid-set-query-timeout.php>`_



*   `cubrid_version <http://www.php.net/manual/en/function.cubrid-version.php>`_



**Note**
To get the latest information about PHP driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-php-driver>`_
.

**PDO Driver**

**PDO Overview**

The official CUBRID PHP Data Objects (PDO) driver is available as a PECL package and it implements the PDO interface to enable access from PDO to CUBRID. PDO is available with PHP 5.1. For PHP 5.0, you can use it as a PECL extension. PDO cannot run with earlier versions of PHP 5.0 because it requires the new OO features in the core of PHP 5.0.

PDO provides a data-access abstraction layer, which means that, regardless of which database you are using, you use the same functions to issue queries and fetch data; PDO does not provide a database abstraction. Using PDO as a database interface layer can have important advantages over "direct" PHP database drivers as follows:

*   Portable PHP code between different databases and database abstraction.



*   Supports SQL parameters and bind.



*   Safer SQLs (syntax verification, escaping, it helps protect against SQL injections etc.)



*   Cleaner programming model



In particular, having a CUBRID PDO driver means that any application that uses PDO as a database interface should work with CUBRID.

CUBRID PDO driver is based on CCI API so affected by CCI configurations such as
**CCI_DEFAULT_AUTOCOMMIT**
.

To download PDO driver or get the latest information, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver>`_
.

**Installing and Configuring PDO**

**Linux**

**Requirements**

*   CUBRID: 2008 R3.0 (8.3.0) or later



*   Operating system: 32-bit or 64-bit Linux



*   Web server: Apache



*   PHP: 5.2 or 5.3 (
    `http://php.net/downloads.php <http://php.net/downloads.php>`_
    )



**Installing CUBRID PHP Driver using PECL**

If
**PECL**
package has bee installed on your system, the installation of CUBRID PDO driver is straightforward.
**PECL**
will download and compile the driver for you. If you do not have
**PECL**
installed, follow the instructions at
`http://www.cubrid.org/wiki_apis/entry/installing-cubrid-php-driver-using-pecl <http://www.cubrid.org/wiki_apis/entry/installing-cubrid-php-driver-using-pecl>`_
to get it installed.

*   Enter the following command to install the latest version of CUBRID PDO driver.



sudo pecl install pdo_cubrid

If you need earlier versions of the driver, you can install exact versions as follows:

sudo pecl install pdo_cubrid-8.3.1.0003

During the installation, you will be prompted to enter
**CUBRID base install dir autodetect :**
. Just to make sure your installation goes smootyly, enter the full path to the directory where you have CUBRID installed. For example, if CUBRID has been installed at
**/home/cubridtest/CUBRID**
, then enter
**/home/cubridtest/CUBRID**
.

*   Edit the configuration file.



*   If you are using CentOS 6.0 and later or Fedora 15 and later, create a file named
    **pdo_cubrid.ini**
    , enter a command line
    **extension=pdo_cubrid.so**
    , and store the file in the
    **/etc/php.d**
    directory.



*   If you are using earlier versions of Cent0S or Fedora 15, edit the
    **php.ini**
    file (default location:
    **/etc/php5/apache2**
    or
    **/etc/**
    ) and add the following two command lines at the end of the file.



[CUBRID]

extension=pdo_cubrid.so

*   Restart the web server to apply changes.



**Windows**

**Requirements**

*   CUBRID: 2008 R3.0 (8.3.0) or later



*   Operating system: 32-bit or 64-bit Windows



*   Web server: Apache or IIS



*   PHP: 5.2 or 5.3 (
    `http://windows.php.net/download/ <http://windows.php.net/download/>`_
    )



**Downloading and Installing Compiled CUBRID PDO Driver**

First, download CUBRID PHP/PDO driver of which versions match the versions of your operating system and PHP installed at
`http://www.cubrid.org/?mid=downloads&item=php_driver&os=windows&ostype=any&php=any&driver_type=pdo <http://www.cubrid.org/?mid=downloads&item=php_driver&os=windows&ostype=any&php=any&driver_type=pdo>`_
.

After you download the driver, you will see the
**php_cubrid.dll**
file for CUBRID PHP driver or the
**php_pdo_cubrid.dll**
file for CUBRID PDO driver. Follow the steps below to install it.

*   Copy this driver to the default PHP extensions directory (usually located at
    **C:\Program Files\PHP\ext**
    ).



*   Set your system environment. Check if the environment variable
    **PHPRC**
    is
    **C:\Program Files\PHP**
    and system variable path is added with
    **%PHPRC%**
    and
    **%PHPRC\ext**
    . 



*   Edit
    **php.ini**
    (
    **C:\Program Files\PHP\php.ini**
    ) and add the following two lines at the end of the
    **php.ini**
    file.



[PHP_PDO_CUBRID]

extension=php_pdo_cubrid.dll

For CUBRID PHP driver, add command lines below.

[PHP_CUBRID]

extension = php_cubrid.dll

*   Restart your web server to apply changes.



**Note**
To get the latest information about PDO driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver>`_
.

**PDO Programming**

**Data Source Name (DSN)**

The PDO_CUBRID data source name (DSN) consists of the following elements:

+-------------+--------------------------------------------------------+
| **Element** | **Description**                                        |
|             |                                                        |
+-------------+--------------------------------------------------------+
| DSN prefix  | The DSN prefix is                                      |
|             | **cubrid**                                             |
|             | .                                                      |
|             |                                                        |
+-------------+--------------------------------------------------------+
| host        | The hostname on which the database server resides      |
|             |                                                        |
+-------------+--------------------------------------------------------+
| port        | The port number where the database server is listening |
|             |                                                        |
+-------------+--------------------------------------------------------+
| dbname      | The name of the database                               |
|             |                                                        |
+-------------+--------------------------------------------------------+

**Example**

"cubrid:host=127.0.0.1;port=33000;dbname=demodb"

**Predefined Constants**

The constants defined by CUBRID PDO driver are available only when the extension has been either compiled into PHP or dynamically loaded at runtime. In addition, these driver-specific constants should only be used if you are using PDO driver. Using driver-specific attributes with another driver may result in unexpected behaviour.

The
`PDO::getAttribute() <http://docs.php.net/manual/en/pdo.getattribute.php>`_
function may be used to obtain the
**PDO_ATTR_DRIVER_NAME**
attribute value to check the driver if your code can run

The constants below can be used with the
`PDO::cubrid_schema <http://www.php.net/manual/en/pdo.cubrid-schema.php>`_
function to get schema information.

+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Constant**                       | **Type** | **Description**                                                                                                                                                                         |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_TABLE              | integer  | Gets name and type of table in CUBRID.                                                                                                                                                  |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_VIEW               | integer  | Gets name and type of view in CUBRID.                                                                                                                                                   |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_QUERY_SPEC         | integer  | Get the query definition of view.                                                                                                                                                       |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_ATTRIBUTE          | integer  | Gets the attributes of table column.                                                                                                                                                    |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_TABLE_ATTRIBUTE    | integer  | Gets the attributes of table.                                                                                                                                                           |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_TABLE_METHOD       | integer  | Gets the instance method. The instance method is a method called by a class instance. It is used more often than the class method because most operations are executed in the instance. |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_METHOD_FILE        | integer  | Gets the information of the file where the method of the table is defined.                                                                                                              |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_SUPER_TABLE        | integer  | Gets the name and type of table which table inherits attributes from.                                                                                                                   |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_SUB_TABLE          | integer  | Gets the name and type of table which inherits attributes from this table.                                                                                                              |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_CONSTRAINT         | integer  | Gets the table constraints.                                                                                                                                                             |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_TRIGGER            | integer  | Gets the table triggers.                                                                                                                                                                |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_TABLE_PRIVILEGE    | integer  | Gets the privilege information of table.                                                                                                                                                |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_COL_PRIVILEGE      | integer  | Gets the privilege information of column.                                                                                                                                               |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_DIRECT_SUPER_TABLE | integer  | Gets the direct super table of table.                                                                                                                                                   |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_DIRECT_PRIMARY_KEY | integer  | Gets the table primary key.                                                                                                                                                             |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_IMPORTED_KEYS      | integer  | Gets imported keys of table.                                                                                                                                                            |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_EXPORTED_KEYS      | integer  | Gets exported keys of table.                                                                                                                                                            |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| PDO::CUBRID_SCH_CROSS_REFERENCE    | integer  | Gets reference relationship of two tables.                                                                                                                                              |
|                                    |          |                                                                                                                                                                                         |
+------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Note**
To get the latest information about PDO driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver>`_
.

**PDO Sample Program**

**Verifying CUBRID PDO Driver Version**

If you want to verify that the CUBRID PDO driver is accessible, you can use the
`PDO::getAvailableDrivers <http://docs.php.net/manual/en/pdo.getavailabledrivers.php>`_
() function.

<?php

echo'PDO Drivers available:

';

foreach(PDO::getAvailableDrivers()as $driver)

{

if($driver =="cubrid"){

echo" - Driver: <b>".$driver.'</b>

';

}else{

echo" - Driver: ".$driver.'

';

}

}

?>

This script will output all the currently installed PDO drivers:

PDO Drivers available:

- Driver: mysql

- Driver: pgsql

- Driver: sqlite

- Driver: sqlite2

- Driver: cubrid

**Connecting to CUBRID**

Use the data source name (DSN) to connect to the database server. For details about DSN, see
`Data Source Name (DSN) <#api_api_pdo_programming_htm_dsn>`_
.

Below is a simple PHP example script which performs a PDO connection to the CUBRID
*demodb*
database. You can notice that errors are handling in PDO by using a try-catch mechanism and the connection is closed by assigning
**NULL**
to the connection object.

<?php

$database ="demodb";

$host ="localhost";

$port ="30000";//use default value

$username ="dba";

$password ="";

 

try{

//cubrid:host=localhost;port=33000;dbname=demodb

$conn_str ="cubrid:dbname=".$database.";host=".$host.";port=".$port;

echo"PDO connect string: ".$conn_str."

";

$db =new PDO($conn_str, $username, $password );

echo"PDO connection created ok!"."

";

$db = null;//disconnect

}catch(PDOException $e){

echo"Error: ".$e->getMessage()."

";

}

?>

If connection succeeds, the output of this script is as follows:

PDO connect string: cubrid:dbname=demodb;host=localhost;port=30000

PDO connection created ok!

**Executing a SELECT Statement**

In PDO, there is more than one way to execute SQL queries.

*   Using the
    `query <http://docs.php.net/manual/en/pdo.exec.php>`_
    () function



*   Using prepared statements (see
    `prepare <http://docs.php.net/manual/en/pdo.prepare.php>`_
    ()/
    `execute <http://docs.php.net/manual/en/pdostatement.execute.php>`_
    ()) functions)



*   Using the
    `exec <http://docs.php.net/manual/en/pdo.exec.php>`_
    () function



The example script below shows the simplest one - using the
`query <http://docs.php.net/manual/en/pdo.exec.php>`_
() function. You can retrieve the return values from the resultset (a PDOStatement object) by using the column names, like $rs["
*column_name*
"].

Note that when you use the
`query <http://docs.php.net/manual/en/pdo.exec.php>`_
() function, you must ensure that the query code is properly escaped. For information about escaping, see the 
`PDO::quote <http://www.php.net/manual/en/pdo.quote.php>`_
() function.

<?php

include("_db_config.php");

include("_db_connect.php");

 

$sql ="SELECT * FROM code";

echo"Executing SQL: <b>".$sql.'</b>

';

echo'

';

 

try{

foreach($db->query($sql)as $row){

echo $row['s_name'].' - '. $row['f_name'].'

';

}

}catch(PDOException $e){

echo $e->getMessage();

}

 

$db = null;//disconnect

?>

The output of the script is as follows:

Executing SQL: SELECT * FROM code

 

X - Mixed

W - Woman

M - Man

B - Bronze

S - Silver

G - Goldie

**Executing an UPDATE Statement**

The following example shows how to execute an UPDATE statement by using a prepared statement and parameters. You can use the
`exec <http://docs.php.net/manual/en/pdo.exec.php>`_
() function as an alternative.

<?php

include("_db_config.php");

include("_db_connect.php");

 

$s_name ='X';

$f_name ='test';

$sql ="UPDATE code SET f_name=:f_name WHERE s_name=:s_name";

 

echo"Executing SQL: <b>".$sql.'</b>

';

echo'

';

 

echo":f_name: <b>".$f_name.'</b>

';

echo'

';

echo":s_name: <b>".$s_name.'</b>

';

echo'

';

 

$qe = $db->prepare($sql);

$qe->execute(array(':s_name'=>$s_name,':f_name'=>$f_name));

 

$sql ="SELECT * FROM code";

echo"Executing SQL: <b>".$sql.'</b>

';

echo'

';

 

try{

foreach($db->query($sql)as $row){

echo $row['s_name'].' - '. $row['f_name'].'

';

}

}catch(PDOException $e){

echo $e->getMessage();

}

 

$db = null;//disconnect

?>

The output of the script is as follows:

Executing SQL: UPDATE code SET f_name=:f_name WHERE s_name=:s_name

 

:f_name: test

 

:s_name: X

 

Executing SQL: SELECT * FROM code

 

X - test

W - Woman

M - Man

B - Bronze

S - Silver

G – Goldie

**Using prepare and bind**

Prepared statements are one of the major features offered by PDO and you can take following benefits by using them.

*   SQL prepared statements need to be parsed only once even if they are executed multiple times with different parameter values. Therefore, using a prepared statement minimizes the resources and ,in general, the prepared statements run faster.



*   It helps to prevent SQL injection attacks by eliminating the need to manually quote the parameters; however, if other parts of the SQL query are being built up with unescaped input, SQL injection would still be possible.



The example script below shows how to retrieve data by using a prepared statement.

<?php

include("_db_config.php");

include("_db_connect.php");

 

$sql ="SELECT * FROM code WHERE s_name NOT LIKE :s_name";

echo"Executing SQL: <b>".$sql.'</b>

';

 

$s_name ='xyz';

echo":s_name: <b>".$s_name.'</b>

';

 

echo'

';

 

try{

$stmt = $db->prepare($sql);

 

$stmt->bindParam(':s_name', $s_name, PDO::PARAM_STR);

$stmt->execute();

 

$result = $stmt->fetchAll();

foreach($result as $row)

{

echo $row['s_name'].' - '. $row['f_name'].'

';

}

}catch(PDOException $e){

echo $e->getMessage();

}

echo'

';

 

$sql ="SELECT * FROM code WHERE s_name NOT LIKE :s_name";

echo"Executing SQL: <b>".$sql.'</b>

';

 

$s_name ='X';

echo":s_name: <b>".$s_name.'</b>

';

 

echo'

';

 

try{

$stmt = $db->prepare($sql);

 

$stmt->bindParam(':s_name', $s_name, PDO::PARAM_STR);

$stmt->execute();

 

$result = $stmt->fetchAll();

foreach($result as $row)

{

echo $row['s_name'].' - '. $row['f_name'].'

';

}

$stmt->closeCursor();

}catch(PDOException $e){

echo $e->getMessage();

}

echo'

';

 

$db = null;//disconnect

?>

The output of the script is as follows:

Executing SQL: SELECT * FROM code WHERE s_name NOT LIKE :s_name

:s_name: xyz

 

X - Mixed

W - Woman

M - Man

B - Bronze

S - Silver

G - Goldie

 

Executing SQL: SELECT * FROM code WHERE s_name NOT LIKE :s_name

:s_name: X

 

W - Woman

M - Man

B - Bronze

S - Silver

G - Goldie

**Using the PDO::getAttribute() Function**

The
`PDO::getAttribute <http://docs.php.net/manual/en/pdo.getattribute.php>`_
() function is very useful to retrieve the database connection attributes. For example,

*   Driver name



*   Database version



*   Auto-commit state



*   Error mode



Note that if you want to set attributes values (assuming that they are writable), you should use the
`PDO::setAttribute <http://docs.php.net/manual/en/pdo.setattribute.php>`_
function.

The following example script shows how to retrieve the current versions of client and server by using the
`PDO::getAttribute <http://docs.php.net/manual/en/pdo.getattribute.php>`_
() function.

<?php

include("_db_config.php");

include("_db_connect.php");

 

echo"Driver name: <b>".$db->getAttribute(PDO::ATTR_DRIVER_NAME)."</b>";

echo"

";

echo"Client version: <b>".$db->getAttribute(PDO::ATTR_CLIENT_VERSION)."</b>";

echo"

";

echo"Server version: <b>".$db->getAttribute(PDO::ATTR_SERVER_VERSION)."</b>";

echo"

";

 

$db = null;//disconnect

?>

The output of the script is as follows:

Driver name: cubrid

Client version: 8.3.0

Server version: 8.3.0.0337

**CUBRID PDO Extensions**

In CUBRID, the
`PDO::cubrid_schema <http://kr.php.net/manual/en/pdo.cubrid-schema.php>`_
() function is offered as an extension; the function is used to retrieve the database schema and metadata information. Below is an example script that returns information about primary key for the
*nation*
table by using the
`PDO::cubrid_schema <http://kr.php.net/manual/en/pdo.cubrid-schema.php>`_
() function.

<?php

include("_db_config.php");

include("_db_connect.php");

try{

echo"Get PRIMARY KEY for table: <b>nation</b>:

 

";

$pk_list = $db->cubrid_schema(PDO::CUBRID_SCH_PRIMARY_KEY,"nation");

print_r($pk_list);

}catch(PDOException $e){

echo $e->getMessage();

}

 

$db = null;//disconnect

?>

The output of the script is as follows:

Get PRIMARY KEY for table: nation:

Array ( [0] => Array ( [CLASS_NAME] => nation [ATTR_NAME] => code [KEY_SEQ] => 1 [KEY_NAME] => pk_nation_code ) )

**Note**
To get the latest information about PDO driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver>`_
.

**PDO API**

For more information about PHP Data Objects (PDO) API, see
`http://docs.php.net/manual/en/book.pdo.php <http://docs.php.net/manual/en/book.pdo.php>`_
. The API provided by CUBRID PDO driver is as follows:

*   `PDO_CUBRID DSN <http://www.php.net/manual/en/ref.pdo-cubrid.connection.php>`_



*   `PDO::cubrid_schema <http://www.php.net/manual/en/pdo.cubrid-schema.php>`_



**Note**
To get the latest information about PDO driver, click
`http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-pdo-driver>`_
.