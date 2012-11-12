**********************
Installing and Running
**********************

**Installing and Running on Linux**

**Details to Check when Installing**

Check the following before installing CUBRID for Linux.

+-----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Category**                | **Description**                                                                                                                                                      |
|                             |                                                                                                                                                                      |
+-----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Operating system            | Only supports glibc 2.3.4 or later.                                                                                                                                  |
|                             | The glibc version can be checked as follows:                                                                                                                         |
|                             | %rpm -q glibc                                                                                                                                                        |
|                             |                                                                                                                                                                      |
+-----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 64-bit                      | Since version 2008 R2.0, CUBRID supports both 32-bit and 64-bit Linux. You can check the version as follows:                                                         |
|                             | % uname -a                                                                                                                                                           |
|                             | Linux host_name 2.6.18-53.1.14.el5xen #1 SMP Wed Mar 5 12:08:17 EST 2008 x86_64 x86_64 x86_64 GNU/Linux                                                              |
|                             | Make sure to install the CUBRID 32-bit version on 32-bit Linux and the CUBRID 64-bit version on 64-bit Linux. The followings are the libraries that should be added. |
|                             | Curses Library (rpm -q ncurses)                                                                                                                                      |
|                             | gcrypt Library (rpm -q libgcrypt)                                                                                                                                    |
|                             | stdc++ Library (rpm -q libstdc++)                                                                                                                                    |
|                             |                                                                                                                                                                      |
+-----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Available free memory space | 1 GB or more recommended.                                                                                                                                            |
|                             |                                                                                                                                                                      |
+-----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Available free disk space   | 2 GB or more recommended (500 MB is required for initial installation and 1.5 GB is required for creating the default option database).                              |
|                             |                                                                                                                                                                      |
+-----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Required software           | The CUBRID Manager and Java stored procedures require the Java Runtime Environment (JRE) version 1.6 or later.                                                       |
|                             |                                                                                                                                                                      |
+-----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Installing CUBRID**

The installation program consists shell scripts that contain binary; thus it can be installed automatically. The following example shows how to install CUBRID with the "CUBRID-9.0.0.0470-linux.x86_64.sh" file on the Linux.

[cub_user@cubrid ~]$ sh CUBRID-9.0.0.0470-linux.x86_64.sh

Do you agree to the above license terms? (yes or no) : yes

Do you want to install this software(CUBRID) to the default(/home1/cub_user/CUBRID) directory? (yes or no) [Default: yes] : yes

Install CUBRID to '/home1/cub_user/CUBRID' ...

In case a different version of the CUBRID product is being used in other machines, please note that the CUBRID 2008 R3.1 servers are only compatible with the CUBRID 2008 R3.1 clients and vice versa.

Do you want to continue? (yes or no) [Default: yes] : yes

Copying old .cubrid.sh to .cubrid.sh.bak ...

 

CUBRID has been successfully installed.

 

demodb has been successfully created.

 

If you want to use CUBRID, run the following commands

  % . /home1/cub_user/.cubrid.sh

  % cubrid service start

As shown in the example above, after installing the downloaded file (CUBRID-9.0.0.0470-linux.x86_64.sh), the CUBRID related environment variables must be set in order to use the CUBRID database. Such setting has been made automatically when logging in the concerned terminal. Therefore there is no need to re-set after the first installation.

[cub_user@cubrid ~]$ . /home1/cub_user/.cubrid.sh

After the CUBRID Manager is installed, you can start the CUBRID Manager server and broker as follows:

[cub_user@cubrid ~]$ cubrid service start

After starting the CUBRID service, if you wish to check whether the service was properly started, then check whether the cub_* processes start with grep (as shown below).

[cub_user@cubrid ~]$ ps -ef | grep cub_

cub_user 15200 1 0 18:57 ? 00:00:00 cub_master

cub_user 15205 1 0 18:57 pts/17 00:00:00 cub_broker

cub_user 15210 1 0 18:57 pts/17 00:00:00 query_editor_cub_cas_1

cub_user 15211 1 0 18:57 pts/17 00:00:00 query_editor_cub_cas_2

cub_user 15212 1 0 18:57 pts/17 00:00:00 query_editor_cub_cas_3

cub_user 15213 1 0 18:57 pts/17 00:00:00 query_editor_cub_cas_4

cub_user 15214 1 0 18:57 pts/17 00:00:00 query_editor_cub_cas_5

cub_user 15217 1 0 18:57 pts/17 00:00:00 cub_broker

cub_user 15222 1 0 18:57 pts/17 00:00:00 broker1_cub_cas_1

cub_user 15223 1 0 18:57 pts/17 00:00:00 broker1_cub_cas_2

cub_user 15224 1 0 18:57 pts/17 00:00:00 broker1_cub_cas_3

cub_user 15225 1 0 18:57 pts/17 00:00:00 broker1_cub_cas_4

cub_user 15226 1 0 18:57 pts/17 00:00:00 broker1_cub_cas_5

cub_user 15229 1 0 18:57 ? 00:00:00 cub_auto start

cub_user 15232 1 0 18:57 ? 00:00:00 cub_js start

**Installing CUBRID (rpm File)**

You can install CUBRID by using rpm file that is created on CentOS5. The way of installing and uninstalling CUBRID is the same as that of using general rpm utility. While CUBRID is being installed, a new system group (cubrid) and a user account (cubrid) are created. After installation is complete, you should log in with a cubrid user account to start a CUBRID service.

$ rpm -Uvh CUBRID-9.0.0.0470-el5.x86_64.rpm

When rmp is executed, CUBRID is installed in the cubrid home directory (/opt/cubrid) and related configuration file (cubrid.[c]sh) is installed in the /etc/profile.d directory. Note that
*demodb*
is not automatically installed. Therefore, you must executed /opt/cubrid/demo/make_cubrid_demo.sh. When installation is complete, enter the code below to start CUBRID.

[cubrid@cubrid ~]$ cubrid service start

**Note 1**
You must check RPM dependency when installing with RPM. If you ignore (--nodeps) dependency, it may not be executed.
Even if you remove RPM, user accounts and databases that are created after installing, you must remove it manually, if needed.

**Note 2**
How to use service or chkconfig command
If you use SH or RPM package to install CUBRID, the cubrid script will be included in the $CUBRID/share/init.d directory. In this file, you can find the environment variable,
**CUBRID_USER**
. If you change this variable to the Linux account with which CUBRID has been installed and register it in /etc/init.d, then you can use service or chkconfig command.

**Installing CUBRID on Fedora/CentOS**

To install CUBRID using the yum command, you should know where the CUBRID package is located. Choose appropriate location based on your operating system.

*   `http://www.cubrid.org/yum_repository <http://www.cubrid.org/yum_repository>`_



For example, if you are using Fedora 16, execute the command below. In the example, fc16 refers to Fedora 16.

$ rpm -i http://yumrepository.cubrid.org/cubrid_repo_settings/9.0.0/cubridrepo-9.0.0-1.fc16.noarch.rpm

If you are using CentOS 6.2, execute the command below. In this example, el6.2 refers to CentOS.

$ rpm -i http://yumrepository.cubrid.org/cubrid_repo_settings/9.0.0/cubridrepo-9.0.0-1.el6.2.noarch.rpm

You can install the CUBRID package you have desired based on the command you execute. To install the latest version, execute the command below.

$ yum install cubrid

To install the earlier version, you should include version information in the command.

$ yum install cubrid-9.0.0

After installation is complete, configure environment variables including installation path of CUBRID and then apply them to system.

**Installing CUBRID on Ubuntu**

To install CUBRID using the apt-get command on Ubuntu, add the CUBRID storage first and then update the apt index.

$ sudo add-apt-repository ppa:cubrid/cubrid

$ sudo apt-get update

To install the latest version, execute the command below.

$ sudo apt-get install cubrid

To install the earlier version, you should include version information in the command.

$ sudo apt-get install cubrid-8.3.1

After installation is complete, configure environment variables including installation path of CUBRID and then apply them to system.

**Upgrading CUBRID**

When you specify an installation directory where the previous version of CUBRID is already installed, a message which asks to overwrite files in the directory will appear. Entering
**no**
will stop the installation.

Directory '/home1/cub_user/CUBRID' exist!

If a CUBRID service is running on this directory, it may be terminated abnormally.

And if you don't have right access permission on this directory(subdirectories or files), install operation will be failed.

Overwrite anyway? (yes or no) [Default: no] : yes

Choose whether to overwrite the existing configuration files during the CUBRID installation. Entering
**yes**
will overwrite and back up them as extension .bak files.

The configuration file (.conf or .pass) already exists. Do you want to overwrite it? (yes or no) : yes

**Configuring Environment**

You can modify the environment such as service ports etc. edit the parameters of a configuration file located in the
**$CUBRID/conf**
directory. See
`Installing and Running on Windows <#gs_gs_install_windows_htm>`_
for more information.

**Installing CUBRID Interfaces**

You can see the latest information on interface modules such as CCI, JDBC, PHP, ODBC, OLE DB, ADO.NET, Ruby, and Python and install them by downloading files from
`http://www.cubrid.org/wiki_apis <http://www.cubrid.org/wiki_apis>`_
.

**Installing CUBRID Tools**

You can see the latest information on tools such as CUBRID Manager and CUBRID Query Browser and install them by downloading files from
`http://www.cubrid.org/wiki_tools <http://www.cubrid.org/wiki_tools>`_
.

**Installing and Running on Windows**

**Details to Check when Install**

CUBRID 2008 R2.0 supports both 32-bit and 64-bit Windows. You can check the version by selecting [My Computer] > [System Properties]. Make sure to install the CUBRID 32-bit version on 32-bit Windows and the CUBRID 64-bit version on 64-bit Windows.

+-----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Category**                | **Description**                                                                                                                                                                                                                                                        |
|                             |                                                                                                                                                                                                                                                                        |
+-----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 64-bit                      | Since version 2008 R2.0, CUBRID supports both 32-bit and 64-bit Windows. You can check the version by selecting [My Computer] > [System Properties]. Make sure to install the CUBRID 32-bit version on 32-bit Windows and the CUBRID 64-bit version on 64-bit Windows. |
|                             |                                                                                                                                                                                                                                                                        |
+-----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Available free memory space | 1 GB or more recommended.                                                                                                                                                                                                                                              |
|                             |                                                                                                                                                                                                                                                                        |
+-----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Available free disk space   | 2 GB or more recommended (500 MB is required for initial installation and 1.5 GB is required for creating the default option database).                                                                                                                                |
|                             |                                                                                                                                                                                                                                                                        |
+-----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Required software           | The CUBRID Manager and Java stored procedures require the Java Runtime Environment (JRE) version 1.6 or later.                                                                                                                                                         |
|                             |                                                                                                                                                                                                                                                                        |
+-----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

If CUBRID Service Tray does not automatically run upon system startup, you should check followings:

*   Go to [Control Panel] > [Administrative Tools] > [Service] and verify whether Task Scheduler has started. If not, start Task Scheduler.



*   Go to [Administrative Tools] > [Task Scheduler] and verify whether CUBRID Service Tray has been registered. If not, register CUBRID Service Tray.



**Setup Type**

*   **Server and Driver Installation**
    : CUBRID Server, CSQL (a command line tool), interface drivers (OLE DB Provider, ODBC, JDBC, C API) are all installed.



*   **Driver Installation**
    : The interface drivers (OLE DB Provider, ODBC, JDBC, C API) are only installed. You can select this type of installation if development or operation is performed by remote connection to the computer in which the CUBRID database server is installed.



**Upgrading CUBRID**

To install a new version of CUBRID in an environment in which a previous version has already been installed, select [CUBRID Service Tray] > [Exit] from the menu to stop currently running services, and then remove the previous version of CUBRID. Note that when you are prompted with "Do you want to delete all the existing version of databases and the configuration files?" you must select "No" to protect the existing databases.

For more information on migrating a database from a previous version to a new version, see
`Migrating Database <#admin_admin_migration_migration__1472>`_
.

**Configuring Environment**

You can change configuration such as service ports to meet the user environment by changing the parameter values of following files which are located in the
**%CUBRID%\conf**
directory. If a firewall has been configured, the ports used in CUBRID need to be opened.

+------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **File**               | **Description**                                                                                                                                                                                                    |
|                        |                                                                                                                                                                                                                    |
+------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **cm.conf**            | A configuration file for CUBRID Manager. The port that the Manager server process uses is called                                                                                                                   |
|                        | **cm_port**                                                                                                                                                                                                        |
|                        | and its default value is                                                                                                                                                                                           |
|                        | **8001**                                                                                                                                                                                                           |
|                        | . Two ports are used and the port number is determined by the                                                                                                                                                      |
|                        | **cm_port**                                                                                                                                                                                                        |
|                        | parameter. If 8001 is specified, 8001 and 8002 (configured number plus 1) ports will be used. For details, see                                                                                                     |
|                        | `CUBRID Manager Manual <http://www.cubrid.org/wiki_tools/entry/cubrid-manager-manual>`_                                                                                                                            |
|                        | .                                                                                                                                                                                                                  |
|                        |                                                                                                                                                                                                                    |
+------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **cubrid.conf**        | A configuration file for server. You can use it to configure the following values: database memory, the number threads based on the number of concurrent users, communication port between broker and server, etc. |
|                        |                                                                                                                                                                                                                    |
|                        | The port that a master process uses is called                                                                                                                                                                      |
|                        | cubrid_port_id                                                                                                                                                                                                     |
|                        | and its default value is                                                                                                                                                                                           |
|                        | 1523                                                                                                                                                                                                               |
|                        | . For details, see                                                                                                                                                                                                 |
|                        | `cubrid.conf Configuration File and Default Parameters <#pm_pm_db_setting_htm>`_                                                                                                                                   |
|                        | .                                                                                                                                                                                                                  |
|                        |                                                                                                                                                                                                                    |
+------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **cubrid_broker.conf** | A configuration file for broker. You can use it to configure the following values: broker port, the number of application servers (CAS), SQL LOG, etc.                                                             |
|                        | The port that a broker uses is called                                                                                                                                                                              |
|                        | **BROKER_PORT**                                                                                                                                                                                                    |
|                        | . A port you see in the                                                                                                                                                                                            |
|                        | drivers such as JDBC is its corresponding broker's port.                                                                                                                                                           |
|                        | **APPL_SERVER_PORT**                                                                                                                                                                                               |
|                        | is a port that a broker application server (CAS) uses and it is added only in Windows. The default value is                                                                                                        |
|                        | **BROKER_PORT**                                                                                                                                                                                                    |
|                        | +1. The number of ports used is the same as the number of CAS, starting from the specified port's number plus 1. For details, see                                                                                  |
|                        | `Parameter by Broker <#pm_pm_broker_one_htm>`_                                                                                                                                                                     |
|                        | .                                                                                                                                                                                                                  |
|                        | The                                                                                                                                                                                                                |
|                        | **CCI_DEFAULT_AUTOCOMMIT**                                                                                                                                                                                         |
|                        | broker parameter is supported since 2008 R4.0. The default value in the version is                                                                                                                                 |
|                        | **OFF**                                                                                                                                                                                                            |
|                        | and it is later changed to                                                                                                                                                                                         |
|                        | **ON**                                                                                                                                                                                                             |
|                        | . Therefore, users who have upgraded from 2008 R4.0 to 2008 R4.1 or later versions should change this value to                                                                                                     |
|                        | **OFF**                                                                                                                                                                                                            |
|                        | or configure the auto-commit mode to                                                                                                                                                                               |
|                        | **OFF**                                                                                                                                                                                                            |
|                        | .                                                                                                                                                                                                                  |
|                        |                                                                                                                                                                                                                    |
+------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Installing CUBRID Interfaces**

You can see the latest information on interface modules such as JDBC, PHP, ODBC, and OLE DB and install them by downloading files from 
`http://www.cubrid.org/wiki_apis <http://www.cubrid.org/wiki_apis>`_
.

**Installing CUBRID Tools**

You can see the latest information on tools such as CUBRID Manager and CUBRID Query Browser and install them by downloading files from
`http://www.cubrid.org/wiki_tools <http://www.cubrid.org/wiki_tools>`_
.