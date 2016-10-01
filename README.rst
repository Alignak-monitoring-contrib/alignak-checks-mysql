Alignak checks package for Mysql
================================

Checks pack for monitoring mysql database server


Installation
------------

From PyPI
~~~~~~~~~
To install the package from PyPI:
::
   pip install alignak-checks-mysql


From source files
~~~~~~~~~~~~~~~~~
To install the package from the source files:
::
   git clone https://github.com/Alignak-monitoring-contrib/alignak-checks-mysql
   cd alignak-checks-mysql
   sudo python setup.py install


Documentation
-------------

Configuration
~~~~~~~~~~~~~

**Note**: this pack embeds the ``check_mysql_health`` script from http://labs.consol.de/lang/en/nagios/check_mysql_health/.
The embedded version is built from the 2.2.2 version but you may install this script by yourself ...

We recommand that you download and install your own available from the web site.
An abstract::

    $ tar xvfz check_mysql_health-2.2.2
    $ cd check_mysql_health-2.2.2
    $ ./configure --prefix=/usr/local/var/libexec/alignak --with-nagios-user=alignak --with-nagios-group=alignak --with-mymodules-dir=/usr/local/var/libexec/alignak --with-mymodules-dyn-dir=/usr/local/var/libexec/alignak
    $ make

    $ make install

    $ make install-root
    $ # This for plugins requiring setuid (check_icmp ...)

**Note**: replace */usr/local/var/libexec/alignak* according to your platform ...

After compilation and installation, the plugins are installed in the */usr/local/libexec* directory.



To use this checks package, you must first install some external plugins.
We recommand that you download and install the Monitoring plugins: https://www.monitoring-plugins.org/download.html

Check if it exists a binary package for your OS distribution rather than compiling and installing from source.
Else, the source installation procedure is explained `here<https://www.monitoring-plugins.org/doc/faq/installation.html>`_.
An abstract::

    $ gzip -dc monitoring-plugins-2.x.tar.gz | tar -xf -
    $ cd monitoring-plugins-2.x
    $ ./configure
    $ make

    $ make install

    $ make install-root
    $ # This for plugins requiring setuid (check_icmp ...)

After compilation and installation, the plugins are installed in the */usr/local/libexec* directory.

Edit the */usr/local/etc/alignak/arbiter/packs/resource.d/mysql.cfg* file and configure the credentials
to access to the mysql server.
::

    #-- MySQL default credentials
    $MYSQLUSER$=root
    $MYSQLPASSWORD$=root


Alignak configuration
~~~~~~~~~~~~~~~~~~~~~

You simply have to tag the concerned hosts with the template `mysql`.
::

    define host{
        use                     mysql
        host_name               my_server
        address                 127.0.0.1
    }

The main `mysql` template declares macros used to configure the launched checks. The default values of these macros listed hereunder can be overriden in each host configuration.
::
   _DOMAIN                          $DOMAIN$
   _DOMAINUSERSHORT                 $DOMAINUSERSHORT$
   _DOMAINUSER                      $_HOSTDOMAIN$\\$_HOSTDOMAINUSERSHORT$
   _DOMAINPASSWORD                  $DOMAINPASSWORD$

   _WINDOWS_DISK_WARN               90
   _WINDOWS_DISK_CRIT               95
   _WINDOWS_EVENT_LOG_WARN          1
   _WINDOWS_EVENT_LOG_CRIT          2
   _WINDOWS_REBOOT_WARN             15min:
   _WINDOWS_REBOOT_CRIT             5min:
   _WINDOWS_MEM_WARN                80
   _WINDOWS_MEM_CRIT                90
   _WINDOWS_ALL_CPU_WARN            80
   _WINDOWS_ALL_CPU_CRIT            90
   _WINDOWS_CPU_WARN                80
   _WINDOWS_CPU_CRIT                90
   _WINDOWS_LOAD_WARN               10
   _WINDOWS_LOAD_CRIT               20
   _WINDOWS_NET_WARN                80
   _WINDOWS_NET_CRIT                90
   _WINDOWS_EXCLUDED_AUTO_SERVICES
   _WINDOWS_AUTO_SERVICES_WARN      0
   _WINDOWS_AUTO_SERVICES_CRIT      1
   _WINDOWS_BIG_PROCESSES_WARN      25

   #Default Network Interface
   _WINDOWS_NETWORK_INTERFACE       Ethernet

   # Now some alert level for a windows host
   _WINDOWS_SHARE_WARN              90
   _WINDOWS_SHARE_CRIT              95


To set a specific value for an host, declare the same macro in the host definition file.
::
   define host{
      use                     windows-wmi
      contact_groups          admins
      host_name               sim-vm
      address                 192.168.0.16

      # Specific values for this host
      # Change warning and critical alerts level for memory
      # Same for CPU, ALL_CPU, DISK, LOAD, NET, ...
      _WINDOWS_MEM_WARN       10
      _WINDOWS_MEM_CRIT       20

      # Exclude some services from automatic start check
      # Use a regexp that matches against the short or long service name as it can be seen in the properties of the service in Windows.
      # The matching services are excluded in the resulting list.
      # Example: (ShortName)|(ShortName)| ... |(ShortName)
      _WINDOWS_EXCLUDED_AUTO_SERVICES (IAStorDataMgrSvc)|(MMCSS)|(ShellHWDetection)|(sppsvc)|(clr_optimization_v4.0.30319_32)
   }


Bugs, issues and contributing
-----------------------------

Contributions to this project are welcome and encouraged ... issues in the project repository are the common way to raise an information.

License
-------

Alignak Pack EXAMPLE is available under the `GPL version 3 license`_.

.. _GPL version 3 license: http://opensource.org/licenses/GPL-3.0