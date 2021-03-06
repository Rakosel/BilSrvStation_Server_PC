Preseed Cut Discr
*************************
| Published by |author|
| Date: *|date|* Time *|time|*
Preseed file for install Debian Server
======================================
|	check debconf-set-selections -c preseed.cfg
|	
|	 https://wiki.debian.org/DebianInstaller/Preseed
|	 https://www.debian.org/releases/stretch/mips/apbs04.html.ru
|	 https://help.ubuntu.com/lts/installation-guide/s390x/apbs04.html
|	 keys https://www.x.org/releases/X11R7.5/doc/input/XKB-Config.html
|	 https://wiki.archlinux.org/title/Xorg_(%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9)/Keyboard_configuration_(%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9)
|	 https://itdraft.ru/2019/12/17/avtomaticheskaya-ustanovka-debian-pri-pomoshhi-preseed/
|	 https://www.debian.org/releases/stretch/example-preseed.txt
Contents of the preconfiguration file 
======================================
Localization
------------
|	 Preseeding only locale sets language, country and locale. 
|	priority 	debconf/priority
|	fb 	debian-installer/framebuffer
|	language 	debian-installer/language
|	country 	debian-installer/country
|	locale 	debian-installer/locale
|	theme 	debian-installer/theme
|	auto 	auto-install/enable
|	classes 	auto-install/classes
|	файловый 	preseed/file
|	url 	preseed/url
|	domain 	netcfg/get_domain
|	hostname    	netcfg/get_hostname
|	interface 	netcfg/choose_interface
|	protocol 	mirror/protocol
|	suite 	mirror/suite
|	modules 	anna/choose_modules
|	recommends 	base-installer/install-recommends
|	tasks 	tasksel:tasksel/first
|	desktop 	tasksel:tasksel/desktop
|	dmraid 	disk-detect/dmraid/enable
|	keymap 	keyboard-configuration/xkb-keymap
|	preseed-md5 	preseed/file/checksum
.. code-block:: bash
	:linenos:

	
The values can also be preseeded individually for greater flexibility
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash
	:linenos:

	d-i debian-installer/language string ru
	d-i debian-installer/country string RU
	d-i debian-installer/locale string ru_RU.UTF-8
|	 Optionally specify additional locales to be generated.
.. code-block:: bash
	:linenos:

	d-i localechooser/supported-locales multiselect en_US.UTF-8, ru_RU.UTF-8
	
Keyboard selection
------------------
|	 https://wiki.archlinux.org/title/Xorg/Keyboard_configuration
|	 /etc/X11/xorg.conf.d/00-keyboard.conf
|	 /usr/share/X11/xkb/rules/base.lst
|	 https://unix.stackexchange.com/questions/167131/how-to-use-the-config-option-of-setxkbmap
.. code-block:: bash
	:linenos:

	d-i keymap select us
	d-i console-keymaps-at/keymap select us
	d-i keyboard-configuration/xkb-keymap select us,ru
|	d-i keyboard-configuration/xkb-keymap pc105
|	d-i keyboard-configuration/xkb-keymap layout us
.. code-block:: bash
	:linenos:

	
	d-i keyboard-configuration/toggle select Alt+Shift
	
	d-i localechooser/shortlist select RU
	d-i console-setup/ask_detect boolean false
	d-i console-setup/layoutcode string ru
	d-i	console-setup/variant	select	Россия
	d-i	console-setup/toggle	select	Alt+Shift
	
|	 d-i keyboard-configuration/toggle select No toggling
.. code-block:: bash
	:linenos:

	
Network configuration
---------------------
|	 Disable network configuration entirely. This is useful for cdrom
|	 installations on non-networked devices where the network questions,
|	 warning and long timeouts are a nuisance.
.. code-block:: bash
	:linenos:

	d-i hw-detect/load_firmware boolean true
	
	d-i netcfg/enable boolean true
	
|	 netcfg will choose an interface that has link if possible. This makes it
|	 skip displaying a list if there is more than one interface.
.. code-block:: bash
	:linenos:

	
|	 To pick a particular interface instead:
|	d-i netcfg/choose_interface select eth1
.. code-block:: bash
	:linenos:

	
|	 To set a different link detection timeout (default is 3 seconds).
|	 Values are interpreted as seconds.
.. code-block:: bash
	:linenos:

	d-i netcfg/link_wait_timeout string 10
	
|	 If you have a slow dhcp server and the installer times out waiting for
|	 it, this might be useful.
.. code-block:: bash
	:linenos:

	d-i netcfg/dhcp_timeout string 60
|	d-i netcfg/dhcpv6_timeout string 60
.. code-block:: bash
	:linenos:

	d-i netcfg/choose_interface select auto
|	 If you prefer to configure the network manually, uncomment this line and
|	 the static network configuration below.
|	d-i netcfg/disable_autoconfig boolean true
.. code-block:: bash
	:linenos:

	
|	 If you want the preconfiguration file to work on systems both with and
|	 without a dhcp server, uncomment these lines and the static network
|	 configuration below.
|	d-i netcfg/dhcp_failed note
|	 Configure network manually
.. code-block:: bash
	:linenos:

	d-i netcfg/dhcp_options select auto
	
Static network configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	
|	 IPv4 example
|	d-i netcfg/get_ipaddress string 192.168.1.42
|	d-i netcfg/get_netmask string 255.255.255.0
|	d-i netcfg/get_gateway string 192.168.1.1
|	d-i netcfg/get_nameservers string 192.168.1.1
|	d-i netcfg/confirm_static boolean true
|	
|	 IPv6 example
|	d-i netcfg/get_ipaddress string fc00::2
|	d-i netcfg/get_netmask string ffff:ffff:ffff:ffff::
|	d-i netcfg/get_gateway string fc00::1
|	d-i netcfg/get_nameservers string fc00::1
|	d-i netcfg/confirm_static boolean true
.. code-block:: bash
	:linenos:

	
|	 Any hostname and domain names assigned from dhcp take precedence over
|	 values set here. However, setting the values still prevents the questions
|	 from being shown, even if values come from dhcp.
|	d-i netcfg/get_hostname string unassigned-hostname
|	d-i netcfg/get_domain string unassigned-domain
.. code-block:: bash
	:linenos:

	
|	 If you want to force a hostname, regardless of what either the DHCP
|	 server returns or what the reverse DNS entry for the IP is, uncomment
|	 and adjust the following line.
.. code-block:: bash
	:linenos:

	d-i netcfg/hostname string BilSrvStation
	
|	 Disable that annoying WEP key dialog.
|	d-i netcfg/wireless_wep string
|	 The wacky dhcp hostname that some ISPs use as a password of sorts.
|	d-i netcfg/dhcp_hostname string BilSrvStation
.. code-block:: bash
	:linenos:

	
|	 If non-free firmware is needed for the network or other hardware, you can
|	 configure the installer to always try to load it, without prompting. Or
|	 change to false to disable asking.
.. code-block:: bash
	:linenos:

	d-i hw-detect/load_firmware boolean true
	
	d-i netcfg/get_domain string BilSrvWork
	d-i netcfg/confirm_static boolean false
	d-i netcfg/disable_dhcp boolean false
Network console
~~~~~~~~~~~~~~~
|	 Use the following settings if you wish to make use of the network-console
|	 component for remote installation over SSH. This only makes sense if you
|	 intend to perform the remainder of the installation manually.
|	d-i anna/choose_modules string network-console
|	d-i network-console/authorized_keys_url string http://10.0.0.1/openssh-key
|	d-i network-console/password password r00tme
|	d-i network-console/password-again password r00tme
.. code-block:: bash
	:linenos:

	
Mirror settings
~~~~~~~~~~~~~~~
|	 If you select ftp, the mirror/country string does not need to be set.
|	d-i mirror/protocol string ftp
.. code-block:: bash
	:linenos:

	d-i mirror/country string manual
	d-i mirror/http/hostname string http.us.debian.org
	d-i mirror/http/directory string /debian
	d-i mirror/http/proxy string
	
Suite to install
----------------
.. code-block:: bash
	:linenos:

	d-i mirror/suite string main
|	 Suite to use for loading installer components (optional).
|	d-i mirror/udeb/suite string testing
.. code-block:: bash
	:linenos:

	
Account setup
~~~~~~~~~~~~~
|	 Skip creation of a root account (normal user account will be able to
|	 use sudo).
.. code-block:: bash
	:linenos:

	d-i passwd/root-login boolean true
|	 Alternatively, to skip creation of a normal user account.
.. code-block:: bash
	:linenos:

	d-i passwd/make-user boolean false
	
Root password
~~~~~~~~~~~~~
|	d-i passwd/root-password password r
|	d-i passwd/root-password-again password r
|	 or encrypted using a crypt(3)  hash.
|	 $ echo "r00tme" | mkpasswd -s -H MD5
.. code-block:: bash
	:linenos:

	d-i passwd/root-password-crypted password $1$7xORR8sC$l1HxV/t6fFCHlYzoobBl./
	
To create a normal user account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	d-i passwd/user-fullname string Debian User
|	d-i passwd/username string debian
Normal user
~~~~~~~~~~~
|	d-i passwd/user-password password insecure
|	d-i passwd/user-password-again password insecure
|	 or encrypted using a crypt(3) hash.
|	d-i passwd/user-password-crypted password [crypt(3) hash]
|	 Create the first user with the specified UID instead of the default.
.. code-block:: bash
	:linenos:

	d-i passwd/user-uid string 1010
	
|	 The user account will be added to some standard initial groups. To
|	 override that, use this.
|	d-i passwd/user-default-groups string audio cdrom video
.. code-block:: bash
	:linenos:

	
Clock and time zone setup
-------------------------
|	 Controls whether or not the hardware clock is set to UTC.
.. code-block:: bash
	:linenos:

	d-i clock-setup/utc boolean true
	
Time/zone
~~~~~~~~~
|	 You may set this to any valid setting for $TZ; see the contents of
|	 /usr/share/zoneinfo/ for valid values.
.. code-block:: bash
	:linenos:

	d-i time/zone string Europe/Saratov
	
NTP-setup
~~~~~~~~~
|	 Controls whether to use NTP to set the clock during the install
.. code-block:: bash
	:linenos:

	d-i clock-setup/ntp boolean true
|	 NTP server to use. The default is almost always fine here.
|	d-i clock-setup/ntp-server string ntp.example.com
.. code-block:: bash
	:linenos:

	
Partitioning
------------
|	|	 Partitioning example
|	 If the system has free space you can choose to only partition that space.
|	 This is only honoured if partman-auto/method (below) is not set.
.. code-block:: bash
	:linenos:

	d-i partman-auto/init_automatically_partition select biggest_free
	
Partman-auto/disk
~~~~~~~~~~~~~~~~~
|	 Alternatively, you may specify a disk to partition. If the system has only
|	 one disk the installer will default to using that, but otherwise the device
|	 name must be given in traditional, non-devfs format (so e.g. /dev/sda
|	 and not e.g. /dev/discs/disc0/disc).
|	 For example, to use the first SCSI/SATA hard disk:
.. code-block:: bash
	:linenos:

	d-i partman-auto/disk string /dev/nvme0n1 /dev/sda 
|	 In addition, you'll need to specify the method to use.
|	 The presently available methods are:
|	 - regular: use the usual partition types for your architecture
|	 - lvm:     use LVM to partition the disk
|	 - crypto:  use LVM within an encrypted partition
.. code-block:: bash
	:linenos:

	d-i partman-auto/method string regular
	
LVM configuration
~~~~~~~~~~~~~~~~~
|	 If one of the disks that are going to be automatically partitioned
|	 contains an old LVM configuration, the user will normally receive a
|	 warning. This can be preseeded away...
.. code-block:: bash
	:linenos:

	d-i partman-lvm/device_remove_lvm boolean true
|	 The same applies to pre-existing software RAID array:
|	d-i partman-md/device_remove_md boolean true
|	 And the same goes for the confirmation to write the lvm partitions.
.. code-block:: bash
	:linenos:

	d-i partman-lvm/confirm boolean true
	d-i partman-lvm/confirm_nooverwrite boolean true
|	 for gpt
.. code-block:: bash
	:linenos:

	d-i partman-basicfilesystems/choose_label string gpt
	d-i partman-basicfilesystems/default_label string gpt
	d-i partman-partitioning/choose_label string gpt
	d-i partman-partitioning/default_label string gpt
	d-i partman/choose_label string gpt
	d-i partman/default_label string gpt
	
You can choose one of the three predefined partitioning recipes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	 - atomic: all files in one partition
|	 - home:   separate /home partition
|	 - multi:  separate /home, /var, and /tmp partitions
.. code-block:: bash
	:linenos:

	d-i partman-auto/choose_recipe select atomic
	
Or provide a recipe of your own
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	 If you have a way to get a recipe file into the d-i environment, you can
|	 just point at it.
|	d-i partman-auto/expert_recipe_file string /hd-media/recipe
.. code-block:: bash
	:linenos:

	
Partitioning using RAID
~~~~~~~~~~~~~~~~~~~~~~~
|	 The method should be set to "raid".
|	d-i partman-auto/method string raid
|	 Specify the disks to be partitioned. They will all get the same layout,
|	 so this will only work if the disks are the same size.
|	partman-auto partman-auto/init_automatically_partition select Guided - use entire disk
|	partman-auto partman-auto/automatically_partition select
.. code-block:: bash
	:linenos:

	
|	 Next you need to specify the physical partitions that will be used. 
|	d-i partman-auto/expert_recipe string \
|	      multiraid ::                                         \
|	              1000 5000 4000 raid                          \
|	                      $primary{ } method{ raid }           \
|	              .                                            \
|	              64 512 300% raid                             \
|	                      method{ raid }                       \
|	              .                                            \
|	              500 10000 1000000000 raid                    \
|	                      method{ raid }                       \
|	              .
.. code-block:: bash
	:linenos:

	
|	 Last you need to specify how the previously defined partitions will be
|	 used in the RAID setup. Remember to use the correct partition numbers
|	 for logical partitions. RAID levels 0, 1, 5, 6 and 10 are supported;
|	 devices are separated using "|	".
|	 Parameters are:
|	 <raidtype> <devcount> <sparecount> <fstype> <mountpoint> \
|	          <devices> <sparedevices>
.. code-block:: bash
	:linenos:

	
|	 https://www.debian.org/releases/stable/amd64/apbs04.en.html
|	 http://vladimir-stupin.blogspot.com/2017/10/using-expertrecipe-debianubuntu.html
|	 https://wikitech.wikimedia.org/wiki/PartMan
|	 https://www.bishnet.net/tim/blog/2015/01/29/understanding-partman-autoexpert_recipe/
|	 https://www.debian.org/releases/stretch/mips/apbs04.html.ru
|	 <raidtype> <devcount> <sparecount> <fstype> <mountpoint> \
|	          <devices> <sparedevices>
|	d-i partman-auto-raid/recipe string \
|	    1 2 0 ext3 /                    \
|	          /dev/sda1|	/dev/sdb1       \
|	    .                               \
|	    1 2 0 swap -                    \
|	          /dev/sda5|	/dev/sdb5       \
|	    .                               \
|	    0 2 0 ext3 /home                \
|	          /dev/sda6|	/dev/sdb6       \
|	    .
.. code-block:: bash
	:linenos:

	d-i partman-auto/expert_recipe string \
	boot-root ::\
	142 100 152 fat32 \
				 $primary{ } $bootable{ } \
	 method{ efi } format{ } \
	 mountpoint{ /boot/efi } \
	 device{/dev/nvme0n1}\
				. \
	8000 8100 8192 linux-swap \
	 method{ swap } format{ } \
	 device{/dev/nvme0n1}\
				. \
				1 1000 -1 ext4 \
	 method{ format } format{ } \
	 use_filesystem{ } filesystem{ ext4 } \
	 mountpoint{ / } \
	 device{/dev/nvme0n1}\
				. 
|	 If not, you can put an entire recipe into the preconfiguration file in one
|	 (logical) line. This example creates a small /boot partition, suitable
|	 swap, and uses the rest of the space for the root partition:
|	d-i partman-auto/expert_recipe string                         \
|	      boot-root ::                                            \
|	              40 50 100 ext3                                  \
|	                      $primary{ } $bootable{ }                \
|	                      method{ format } format{ }              \
|	                      use_filesystem{ } filesystem{ ext3 }    \
|	                      mountpoint{ /boot }                     \
|	              .                                               \
|	              500 10000 1000000000 ext3                       \
|	                      method{ format } format{ }              \
|	                      use_filesystem{ } filesystem{ ext3 }    \
|	                      mountpoint{ / }                         \
|	              .                                               \
|	              64 512 300% linux-swap                          \
|	                      method{ swap } format{ }                \
|	              .
.. code-block:: bash
	:linenos:

	
|	 The full recipe format is documented in the file partman-auto-recipe.txt
|	 included in the 'debian-installer' package or available from D-I source
|	 repository. This also documents how to specify settings such as file
|	 system labels, volume group names and which physical devices to include
|	 in a volume group.
.. code-block:: bash
	:linenos:

	
Partman automatically partition without confirmation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	 This makes partman automatically partition without confirmation, provided
|	 that you told it what to do using one of the methods above.
.. code-block:: bash
	:linenos:

	d-i partman-partitioning/confirm_write_new_label boolean true
	d-i partman/choose_partition select finish
	d-i partman/confirm boolean true
	d-i partman/confirm_nooverwrite boolean true
	
|	 When disk encryption is enabled, skip wiping the partitions beforehand.
|	d-i partman-auto-crypto/erase_disks boolean false
|	 For additional information see the file partman-auto-raid-recipe.txt
|	 included in the 'debian-installer' package or available from D-I source
|	 repository.
|	 This makes partman automatically partition without confirmation.
.. code-block:: bash
	:linenos:

	
Controlling how partitions are mounted
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	 The default is to mount by UUID, but you can also choose "traditional" to
|	 use traditional device names, or "label" to try filesystem labels before
|	 falling back to UUIDs.
|	d-i partman/mount_style select uuid
.. code-block:: bash
	:linenos:

	
Base system installation
------------------------
|	 Configure APT to not install recommended packages by default. Use of this
|	 option can result in an incomplete system and should only be used by very
|	 experienced users.
.. code-block:: bash
	:linenos:

	d-i base-installer/install-recommends boolean false
	
|	 The kernel image (meta) package to be installed; "none" can be used if no
|	 kernel is to be installed.
|	d-i base-installer/kernel/image string linux-image-686
.. code-block:: bash
	:linenos:

	
 Apt setup
~~~~~~~~~~
|	 You can choose to install non-free and contrib software.
.. code-block:: bash
	:linenos:

	d-i apt-setup/non-free boolean true
	d-i apt-setup/contrib boolean true
Uncomment this if you don
~~~~~~~~~~~~~~~~~~~~~~~~~
|	d-i apt-setup/use_mirror boolean false
|	 Select which update services to use; define the mirrors to be used.
|	 Values shown below are the normal defaults.
.. code-block:: bash
	:linenos:

	d-i apt-setup/services-select multiselect security, updates
	d-i apt-setup/security_host string security.debian.org
	
|	 Некоторые версии программы установки могут отсылать отчёт
|	 об установленных и используемых пакетах. По умолчанию данная
|	 возможность выключена, но отправка отчёта помогает проекту
|	 определить популярность программ и какие из них включать на CD.
|	 не отправляем данные об установленных пакетах. 
.. code-block:: bash
	:linenos:

	popularity-contest popularity-contest/participate boolean false
	
Additional repositories
~~~~~~~~~~~~~~~~~~~~~~~
|	d-i apt-setup/local0/repository string \
|	       http://local.server/debian stable main
|	d-i apt-setup/local0/comment string local server
|	 Enable deb-src lines
|	d-i apt-setup/local0/source boolean true
|	 URL to the public key of the local repository; you must provide a key or
|	 apt will complain about the unauthenticated repository and so the
|	 sources.list line will be left commented out
|	d-i apt-setup/local0/key string http://local.server/key
.. code-block:: bash
	:linenos:

	
|	 By default the installer requires that repositories be authenticated
|	 using a known gpg key. This setting can be used to disable that
|	 authentication. Warning: Insecure, not recommended.
|	d-i debian-installer/allow_unauthenticated boolean true
.. code-block:: bash
	:linenos:

	
|	 Uncomment this to add multiarch configuration for i386
|	d-i apt-setup/multiarch string i386
.. code-block:: bash
	:linenos:

	
Package selection
~~~~~~~~~~~~~~~~~
.. code-block:: bash
	:linenos:

	tasksel tasksel/first multiselect none
	
Individual additional packages to install
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	d-i pkgsel/include string openssh-server build-essential
|	 Whether to upgrade packages after debootstrap.
|	 Allowed values: none, safe-upgrade, full-upgrade
.. code-block:: bash
	:linenos:

	d-i pkgsel/upgrade select none
	
|	 Some versions of the installer can report back on what software you have
|	 installed, and what software you use. The default is not to report back,
|	 but sending reports helps the project determine what software is most
|	 popular and include it on CDs.
|	popularity-contest popularity-contest/participate boolean false
.. code-block:: bash
	:linenos:

	
Boot loader installation
~~~~~~~~~~~~~~~~~~~~~~~~
|	 Grub is the default boot loader (for x86). If you want lilo installed
|	 instead, uncomment this:
|	d-i grub-installer/skip boolean true
|	 To also skip installing lilo, and install no bootloader, uncomment this
|	 too:
|	d-i lilo-installer/skip boolean true
.. code-block:: bash
	:linenos:

	
|	 This is fairly safe to set, it makes grub install automatically to the MBR
|	 if no other operating system is detected on the machine.
.. code-block:: bash
	:linenos:

	d-i grub-installer/only_debian boolean true
	
|	 This one makes grub-installer install to the MBR if it also finds some other
|	 OS, which is less safe as it might not be able to boot that other OS.
.. code-block:: bash
	:linenos:

	d-i grub-installer/with_other_os boolean true
	
|	 Due notably to potential USB sticks, the location of the MBR can not be
|	 determined safely in general, so this needs to be specified:
.. code-block:: bash
	:linenos:

	d-i grub-installer/bootdev string default
|	 To install to the first device (assuming it is not a USB stick):
|	d-i grub-installer/bootdev  string default
.. code-block:: bash
	:linenos:

	
|	 Alternatively, if you want to install to a location other than the mbr,
|	 uncomment and edit these lines:
|	d-i grub-installer/only_debian boolean false
|	d-i grub-installer/with_other_os boolean false
|	d-i grub-installer/bootdev  string (hd0,1)
|	 To install grub to multiple disks:
|	d-i grub-installer/bootdev  string (hd0,1) (hd1,1) (hd2,1)
.. code-block:: bash
	:linenos:

	
Optional password for grub
~~~~~~~~~~~~~~~~~~~~~~~~~~
|	d-i grub-installer/password password r00tme
|	d-i grub-installer/password-again password r00tme
|	 or encrypted using an MD5 hash, see grub-md5-crypt(8).
|	d-i grub-installer/password-crypted password [MD5 hash]
.. code-block:: bash
	:linenos:

	
|	 Use the following option to add additional boot parameters for the
|	 installed system (if supported by the bootloader installer).
|	 Note: options passed to the installer will be added automatically.
.. code-block:: bash
	:linenos:

	d-i debian-installer/add-kernel-opts string nousb
	
Finishing up the installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	 During installations from serial console, the regular virtual consoles
|	 (VT1-VT6) are normally disabled in /etc/inittab. Uncomment the next
|	 line to prevent this.
|	d-i finish-install/keep-consoles boolean true
.. code-block:: bash
	:linenos:

	
Avoid that last message about the install being complete
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|	d-i finish-install/reboot_in_progress note
.. code-block:: bash
	:linenos:

	
|	 This will prevent the installer from ejecting the CD during the reboot,
|	 which is useful in some situations.
|	d-i cdrom-detect/eject boolean false
.. code-block:: bash
	:linenos:

	
|	 This is how to make the installer shutdown when finished, but not
|	 reboot into the installed system.
|	d-i debian-installer/exit/halt boolean true
|	 This will power off the machine instead of just halting it.
|	d-i debian-installer/exit/poweroff boolean true
.. code-block:: bash
	:linenos:

	
Preseeding other packages
~~~~~~~~~~~~~~~~~~~~~~~~~
|	 Depending on what software you choose to install, or if things go wrong
|	 during the installation process, it's possible that other questions may
|	 be asked. You can preseed those too, of course. To get a list of every
|	 possible question that could be asked during an install, do an
|	 installation, and then run these commands:
|	   debconf-get-selections --installer > file
|	   debconf-get-selections >> file
.. code-block:: bash
	:linenos:

	d-i finish-install/reboot_in_progress note
	
Advanced options
~~~~~~~~~~~~~~~~
Running custom commands during the installation
"""""""""""""""""""""""""""""""""""""""""""""""
|	 d-i preseeding is inherently not secure. Nothing in the installer checks
|	 for attempts at buffer overflows or other exploits of the values of a
|	 preconfiguration file like this one. Only use preconfiguration files from
|	 trusted locations! To drive that home, and because it's generally useful,
|	 here's a way to run any shell command you'd like inside the installer,
|	 automatically.
.. code-block:: bash
	:linenos:

	
|	 This first command is run as early as possible, just after
|	 preseeding is read.
|	d-i preseed/early_command string anna-install some-udeb
|	 This command is run immediately before the partitioner starts. It may be
|	 useful to apply dynamic partitioner preseeding that depends on the state
|	 of the disks (which may not be visible when preseed/early_command runs).
|	d-i partman/early_command \
|	       string debconf-set partman-auto/disk "$(list-devices disk | head -n1)"
|	 This command is run just before the install finishes, but when there is
|	 still a usable /target directory. You can chroot to /target and use it
|	 directly, or use the apt-install and in-target commands to easily install
|	 packages and run commands in the target system.
|	d-i preseed/late_command string apt-install zsh; in-target chsh -s /bin/zsh 
.. code-block:: bash
	:linenos:

	
|	d-i apt-setup/services-select multiselect security
|	d-i apt-setup/security_host string security.ubuntu.comd
|	d-i apt-setup/security_path string /ubuntu
.. code-block:: bash
	:linenos:

	
|	d-i pkgsel/include string openssh-server build-essential
|	d-i pkgsel/upgrade select full-upgrade
.. code-block:: bash
	:linenos:

	
|	d-i preseed/run string mkdir /target/install/; cp -R /install/* /target/install/; chroot /target chmod +x /install/postinstall.sh; chroot /target; sh /install/postinstall.sh
|	d-i preseed/late_command string \
|	    in-target bash /install/postinstall.sh
.. code-block:: bash
	:linenos:

	
Certificate and postinstall
"""""""""""""""""""""""""""
.. code-block:: bash
	:linenos:

	d-i preseed/late_command string mkdir -p /target/install/; cp -Rf /install/* /target/install/;cp -Rf /cdrom/install_ext/* /target/install/; 
|	in-target apt-get install setools policycoreutils selinux-basics selinux-utils selinux-policy-default selinux-policy-mls auditd policycoreutils-python-utils semanage-utils policycoreutils-gui \
|	in-target selinux-activate
.. code-block:: bash
	:linenos:

	
|	chmod +x /target/install/pii2.sh; cp /target/install/pii2.sh /target/etc/init.d/;  chkconfig --add /target/install/pii2.sh; update-rc.d pii2.sh defaults;
|	d-i preseed/late_command \
|	in-target chmod +x /install/postinstall.sh
|	chroot /target; chmod +x /install/postinstall.sh 
|	\
|	  in-target mkdir -p /root/.ssh; \
|	  in-target chmod 700 /root/.ssh; \
|	  in-target chmod 600 /root/.ssh/authorized_keys; \
|	  in-target mkdir -p /home/myuser/.ssh; \
|	  in-target chmod 700 /home/myuser/.ssh; \
|	  in-target chmod 600 /home/myuser/.ssh/authorized_keys; \
|	  in-target chown -R myuser:myuser /home/myuser/.ssh/; \
|	  in-target mkdir -p /install/; \
|	   cp -R /cdrom/install/* /install/; 
|	  in-target chroot /install; \
|	  in-target chmod +x /install/postinstall.sh 
|	  in-target chroot /install; \ 
|	d-i preseed/late_command string \
|	in-target sh /install/postinstall.sh 
|	d-i preseed/late_command string \
|	  in-target echo -e "o\nn\np\n1\n\n\nw" | fdisk /dev/sdb1 ; \
|	  in-target mkfs.ext4 /dev/sdb1 ; \
|	  in-target echo "/dev/sdb1  /srv  ext4  nodiratime  0  2" >> /etc/fstab
|	<==---END---==>
