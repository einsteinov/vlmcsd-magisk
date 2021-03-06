# vlmcsd

**vlmcsd** - portable open-source KMS Emulator in C

- Supported operating systems / run-time environments
    - Linux, GNU/Linux, uclibc/Linux, musl/Linux, Android (bionic/Linux), FreeBSD, FreeBSD with glibc (e.g. debian/kFreeBSD), OpenBSD, NetBSD, DragonflyBSD, Solaris, Open Indiana, Dyson, Minix, Darwin, Mac OS, iOS, Windows, Cygwin, WSL, Wine, The Hurd.

- Supported CPUs
    - x86, arm, mips, PowerPC, Sparc, s390

### Intro

vlmcsd is

- a replacement for Microsoft's KMS server
- It contains vlmcs, a KMS test client, mainly for debugging purposes, that also can "charge" a genuine KMS server
- designed to run on an always-on or often-on device, e.g. router, NAS Box, ...
- intended to help people who lost activation of their legally-owned licenses, e.g. due to a change of hardware (motherboard, CPU, ...)

vlmcsd is **not**

- a one-click activation or crack tool
- intended to activate illegal copies of software (Windows, Office, Project, Visio)

### Downloads

- Source and binaries: [http://rgho.st/8hXKrr9Dd](http://rgho.st/8hXKrr9Dd)
- Source only: [http://rgho.st/8z9BVlkVC](http://rgho.st/8z9BVlkVC)

7zip password is 2017

### Support

- [Emulated KMS Servers on non-Windows platforms](https://forums.mydigitallife.info/threads/50234-Emulated-KMS-Servers-on-non-Windows-platforms)
- [vlmcsd Magisk module](https://github.com/laggardkernel/vlmcsd-magisk)

### Credit

- All credit belongs to [Hotbird64](https://forums.mydigitallife.info/members/333466-Hotbird64) from MyDigitalLife for the open-source KMS Emulator.
- Magisk module by [laggardkernel](https://github.com/laggardkernel) @Github.

### Changelog

- 2017-01-19 (**1108**)
    - Fixed a bug that Android x86/x64 builds terminated with SIGILL (illegal instruction) if run from a non-Atom CPU (thx to Daz)
    - New option **-x1** (requested by [Hahu](https://forums.mydigitallife.info/members/518557-Hahu)) that causes vlmcsd to exit if
        - any of the listening sockets (IP address / port) specified with **-L** cannot be setup.
        - the TAP mirror thread encounters an error reading from or writing to the VPN adapter.

    - New INI file directive **ExitLevel = 1** that does the same as **-x1**

- 2016-12-12 (**1107**)
    - Fixed a bug in all Cygwin builds that vlmcsd could not be properly stopped if it was started as a service.
    - Updated *./src/GNUmakefile* to include CFLAG *NO_TAP* in *FEATURES=minimum*
    - Updated *GNUmakefile* help to explain CFLAG *NO_TAP*
    - Fixed a bug in all Windows and Cygwin builds with TCP port locking
    - Worked around a problem with POSIX 2008 compatibility in the bionic library of Android 1.5 builds that realpath is unable to allocate a string buffer.

- 2016-12-06 (**1106**)
    - Windows and Cygwin versions of vlmcsd have new command line option **-O** to use an OpenVPN Tap or TeamViewer VPN adapter for easy self-activation
    - New INI file directive **VPN=** (same as command line option **-O**)
    - New CFLAG *NO_TAP* that compiles vlmcsd without support for a TAP or TeamViewer VPN adapter.
    - Fixed a bug that time spans in command line options **-O**, **-A** and **-R** were misinterpreted in rare cases.

- 2016-11-28 (**1105**)
    - Worked around a bug in older uclibc (0.9.30.x) that realpath is not POSIX 2008 compliant (thx to [alorsnon](https://forums.mydigitallife.info/members/164569-alorsnon)).
    - New binaries for the PowerPC architecture using Linux and musl.

- 2016-11-26 (**1104**)
    - vlmcsd now supports loading a KMS product database from file (external database)
        - vlmcsd pre-built binaries now only include a basic database which allows them to perform all tasks as usual. However, if you log, all product names show as "Unknown". For logging with clear text names instead of just GUIDs you'll need an external database. You'll find a full product database in *./etc/vlmcsd.kmd*
        - pre-built binaries of vlmcsd and vlmcs look for an external database in a file named *vlmcsd.kmd* in the same directory as vlmcsd and vlmcs. If it is found, the database is loaded automatically on program start up. If you want a full database (for logging), **just put *vlmcsd.kmd* in the same directory as your vlmcsd binary**. On OpenBSD, Minix and The Hurd systems, vlmcsd and vlmcs look for /etc/vlmcsd.kmd instead.
        - New command line option **-j <filename\>** in vlmcs and vlmcsd to load a specific external database. **-j-** disables loading a default database.
        - New INI file parameter **KmsData = <filename\>** which does the same as command line option **-j**.
        - New CFLAG *-DNO_EXTERNAL_DATA* to compile vlmcsd without support for loading an external database.
        - New CFLAG *-DNO_INTERNAL_DATA* to compile vlmcsd without an internal database at all. vlmcsd will fail, if it doesn't find an external database.
        - New make command line option *DATA=<filename\>* to compile vlmcsd with a custom default location for an external database, e.g. *DATA=/usr/local/etc/vlmcsd*
        - New CFLAG *-DUNSAFE_DATA_LOAD* to compile vlmcsd without checking an external database for integrity. This safes some bytes but you risk remote code execution if you load a specially crafted database. It is strongly recommended not use this CFLAG.
        - New CFLAG *-DFULL_INTERNAL_DATA* to compile vlmcsd with a full internal database. If you combine *-DFULL_INTERNAL_DATABASE* and *-DNO_EXTERNAL_DATA*, your resulting binary will work like previous versions of vlmcsd.
        - CFLAGs *-DNO_EXTENDED_PRODUCT_LIST* and *-DNO_BASIC_PRODUCT_LIST* have been removed.
        - make command line options *WINDOWS=* and *OFFICE201X=* have been removed.
        - A custom database can be created with [License Manager 4.5](https://forums.mydigitallife.info/threads/52594-Miscellaneous-KMS-related-developments?p=1295770&viewfull=1#post1295770) or higher.

    - Fixed a bug in ./src/GNUmakefile that CFLAGs -DNO_STRICT_MODES and -DNO_CLIENT_LIST were not automatically defined, if FEATURES=minimum was specified on the make command line or libkms was built.
    - Fixed a bug that the availability of shared memory was not properly detected in Android builds.
    - vlmcs now includes more clear-text error messages if the KMS server reports an error.
    - The Windows version of vlmcs will now convert all error messages from a KMS server to clear-text.
    - In non-verbose logs vlmcsd will now display the SKU ID instead of 'Unknown' (requested by mkuba50)
    - The Atheros OpenWRT pre-built binaries now support limiting concurrent clients in vlmcsd with command line option -m.
    - The Linux x32 binaries now link with bfd instead of gold which makes them smaller.
    - Fixed a bug that the Windows version of vlmcsd did not lock the listening port if vlmcsd was compiled with CFLAG SIMPLE_SOCKETS. This also affected libkms32.dll and libkms64.dll.
    - Removed unecessary socket shutdown calls in vlmcsd.
    - The RPC code of vlmcsd has been hardened against emulator detection
        - vlmcsd now checks the Interface UUID during RPC binding and sends a proper NACK CTX results.
        - vlmcsd now returns a proper RPC_PT_FAULT package if a client tries to call an unbound CTX index.

    - Fixed a bug that vlmcsd complained about wrong Ndr64 Ctx even if Ndr64 is not used if vlmcsd was compiled with CFLAG -D_PEDANTIC.
    - You can now specify AUXV=1 on the make command line for any toolchain that defines __GLIBC__. In previous versions, AUXV=1 was ignored if __linux__ wasn't defined by the toolchain.

- 2016-11-04 (**1103**)
    - New binary vlmcsd-armelv7-bcm47xx_53xx-openwrt-musl to support OpenWRT on ARM based Broadcom 47xx to 53xx chips
    - New binary vlmcsd-armelv7-bcm47xx_53xx-openwrt-musl-static (same as above as a static build)
    - New binary vlmcsd-armelv7-bcm47xx_53xx-openwrt-glibc for glibc based OpenWRT builds
    - Changed version control system from subversion to git
    - Fixed a bug that -c1 could fail on some Linux builds (due to implicit definition of llabs)
    - Included forgotten CFLAG -DNO_CLIENT_LIST in GNUmakefile help
    - Replaced default start up code in the Visual C++ builds of vlmcsd, vlmcs and vlmcsdmulti with own code (smaller binaries)
    - Visual C++ builds of vlmcsd, vlmcs and vlmcsdmulti now run under XP (SP3 required) and Windows Server 2003

- 2016-10-25 (**svn1099**)
    - Fixed a bug that vlmcs could not send more than request to servers that do not support NDR64.
    - vlmcsd now supports maintaing a list of clients (CMIDs) for better prevention of emulator detection (can be disabled at compile time using new CFLAG -DNO_CLIENT_LISTS).
    - CFLAG -DNO_STRICT_MODES now automatically includes -DNO_CLIENT_LISTS.
    - New vlmcsd command line options -M0 (default) and -M1 to disable or enable maintaining CMIDs.
    - New vlmcsd command line options -E0 (default) and -E1 to disable or enable starting with an empty CMID list. Ignored if -M0 is used.
    - New INI file parameter MaintainClients (same as -M0 and -M1)
    - New INI file parameter StartEmpty (same as -E0 and -E1)
    - Improved auto detection for availability of Posix threads
    - Fixed a bug in the cygwin version of vlmcsd that the logging mutex was not initialized.
    - Fixed a bug in vlmcsd that a request that was too long was not correctly detected.
    - The syslinux.cfg file of the bootable floppy has a new parameter VLMCSD_EXTRA_ARGS to specify additional command line options that will be passed to vlmcsd (requested by vactis).
    - The bootable floppy now uses the threads version of vlmcsdmulti to be able to use -M1 (stripped down kernel has no shared memory support).
    - Fixed a bug in the Linux build script that some vlmcsdmulti binaries were not built correctly.
    - Fixed a typo in the sample vlmcsd.ini file (thx to qewlpal).
    - Changed the default HWID to current Ratiborus virtual machine.
    - The OpenBSD pre-compiled binary now uses threads instead of fork because of missing pthread_mutexattr_setpshared.

- 2016-10-21 (**svn1085**)
    - New CFLAG -DSIMPLE_RPC which disables support for NDR64 and BTFN in the RPC protocol
    - NDR64 and BTFN can now be turned off and on seperately for client and server (required by libkms)
    - New command line options -c0 and -c1 for vlmcsd. -c1 causes to refuse activation if the time of the server and the client differs by more than 4 hours. This is useful for protection against emulator detection.
    - New INI file parameter CheckClientTime (same effect as -c0 and -c1)
    - Renamed CFLAG -DNO_WITHELISTING to -DNO_STRICT_MODES. It disables whitelisting and time checking.
    - Fixed a bug that vlmcsd and vlmcs crashed on CPUs with alignment requirements (PowerPC64, mips64 and sparc64) in 64-bit mode

- 2016-10-19 (**svn1080**)
    - Fixed a bug that Office 2013 preview was not detected as a beta/preview product (thx to Aty).
    - Fixed a bug that the AppList was not compiled in some szenarios where it is actually needed.
    - Fixed a bug that a function in output.c was compiled even if not needed (e.g. NO_LOG defined).

- 2016-10-19 (**svn1079**)
    - vlmcs, vlmcsd and libkms no longer report unspecific errors like !0 but a correct error number that can be converted to a readable string using platform specific APIs (strerror, FormatMessage, etc.)
    - Fixed a bug that vlmcsd could not be compiled with SIMPLE_SOCKETS on systems that do not support IPv6 (e.g. Minix)
    - Visual Studio Linux projects have been temporarily moved to ./src. This is due to a bug in the Visual C++ for Linux extension.
    - correctly set a seperate obj dierectory for each project in Visual Studio projects.
    - Updated DragonFlyBSD build system to version 4.6
    - Updated gcc to version 6.2 for DragonFlyBSD builds.
    - vlmcsd now reports at least twice as much Active Clients as the N Count Policy in the request to make emulator detection more difficult.
    - New command line options -K0, -K1, -K2 and -K3 in vlmcsd that allow whitelisting of KMS IDs. vlmcsd can now refuse unknown KMS IDs, KMS IDs with incorrect App IDs, retail KMS IDs (e.g. Windows Home Editions) and beta/preview KMS IDs (requested by Carlos Detweiller)
    - New INI file parameter WhitelistingLevel (same as -K0, -K1, -K2 and -K3)
    - New CFLAG -DNO_WHITELISTING to compile vlmcsd without the whitelisting feature.
    - libkms now supports RPC feature selection and checking (BTFN, NDR64) during RPC bind. Will be used in future versions of License Manager for advanced emulator detection.

- 2016-10-12 (**svn1065**)
    - Fixed a bug in MSVC builds that caused incorrect parsing of negative integer numbers.
    - MSVC builds now include the compiler version when using option -V in vlmcs and vlmcsd.
    - Fixed a bug in all Windows builds of vlmcsd that -m accepted a maximum value of 32767.
    - Added support for Visual Studio remote building and debugging on Linux (Requires either Visual Studion "15" preview 5 or Visual Studio 2015 with the Linux C++ extension)
    - New improved directory structure
    - All source files are in ./src
    - man file are in ./man
    - All executables go to ./bin by default
    - All libraries go to ./lib
    - Temporary files will be created in ./build (execpt *.d files if you use DEPENDENCIES=1. These must be created in ./src for dependencies to work)
    - Sample vlmcsd.ini is now in ./etc
    - GNUmakefile now always accepts generic names vlmcs, vlmcsd, vlmcsdmulti, libkms and libkms-static as a target name even if *_NAME variables are set
    - Marked all MSVC binaries to safely run from CD and Network and marked them as Terminal Server aware
    - Fixed build warnings with incorrect order of inclusion of windows.h and winsock2.h
    - Fixed a build bug that the -fPIC compiler was not automatically set if you build libkms on systems that use ELF binaries
    - Fixed a bug that caused libkms did not build correctly if VERBOSE=1 was specified on the make command line
    - The FreeBSD build environment was upgraded to FreeBSD 11.0.
    - NetBSD 64-bit binaries now compile with gcc 6.2
    - Updated GNU Hurd compiler to gcc 6.2
    - kFreeBSD compiler updated to 6.2

- 2016-10-06 (**svn1031**)
    - Swapped SKU IDs for Win 2016 Azure Core and Cloud Storage because I trust the pkeyconfig files more than the *.cilx files but still not 100 % sure which is which.
    - vlmcsd, vlmcs, vlmcsdmulti and libkms now compile with Visual Studio 2013 and 2015
    - Added Visual Studio 2015 solution
    - Updated Windows build script to build Visual Studio versions
    - Fixed a bug in vlmcsd that Office 2016 reported 50 active clients instead of 10
    - Windows versions of vlmcsd, vlmcs, vlmcsdmulti and libkms now built with Visual C++ (gcc versions still available with suffix -gcc.exe, all msrpc versions still built with gcc)
    - Updated Mac OS X to Sierra and build tools to Xcode 8.0, clang 3.8, gcc 6.2.0

- 2016-09-16 (**svn1016**)
    - If vlmcsd is compiled with -DNO_LOG, no product names will be included in the binary
    - Some internal changes to easier maintain EPIDs and PKEYCONFIGs
    - The release date of Windows 2016 is now the minimum date generated in random EPIDs
    - New compile time CFLAG -DSMALL_AES that implements a smaller but much slower version of AES
    - It is now safe again to use -DNO_BASIC_PRODUCT_LIST when compiling vlmcsd. Those KMS IDs required to send a proper EPID will always be included in the binary.
    - The APP ID list will no longer be compiled, if -DNO_LOG is used since it is no longer required because EPID generation now uses KMS IDs instead of APP IDs
    - Fixed a bug in vlmcs that incorrect HMACs in KMSv6 were not detected (Thx to CODYQX4)
    - Updated FreeBSD toolchain to gcc 6.1

- 2016-09-03 (**svn1006**)
    - Changed the GNUmakefile to build Windows versions on Windows after Cygwin's upgrade to MingW32 gcc 5.4 based toolchain
    - vlmcsd now uses a seperate EPID for Office 2016 (Thx to Aty and echo2)
    - New command line option -6 in vlmcsd to specify a user-defined EPID for Office 2016
    - The INI file no longer uses GUIDs to define custom EPIDs and HWIDs. Use keywords Windows, Office2010, Office2013 and Office2016 instead.
    - Fixed a bug in the -G option of vlmcs that wrong requests were sent to the KMS server
    - The -G option in vlmcs now uses the new INI file format for creating/updating INI files
    - The GNUmakefile command line parameter FEATURES=minimum no longer includes -DNO_BASIC_PRODUCT_LIST because KMS IDs are now required to distinguish between Office 2013 and Office 2016
    - New GNUmakefile command line option OFFICE2016= to specify a default EPID for Office2016 at compile time
    - If -DNO_BASIC_PRODUCT_LIST is used in GNUmakefile CFLAGS, the resulting binary is unable to send valid EPIDs for Office 2016. This compile time option should no longer be used.
    - The bootable floppy now accepts a kernel boot parameter of OFFICE2016= to specify an EPID for Office 2016

- 2016-08-27 (**svn1003**)
    - Added new KMS ID "Windows 10 2016 (Volume)" (thx to kelorgo and Aty)
    - Moved Windows 10 Enterprise 2016 LTSB and LTSB N to "Windows 10 2016 (Volume)"

- 2016-08-26 (**svn1001**)
    - Added some more products (SKU IDs and KMS IDs)
    - Fixed a critical bug in random ePID generation that ePIDs were generated that never would activate Office 2016, Windows 10, Windows 8.1, Windows 2012 R2 and Windows 2016 on a genuine server.

- 2016-08-11 (**svn998**)
    - libkms now includes an API for KMS clients
    - Fixed a bug in GNUmakefile that libkms did not build without CAT=2 flag
    - The MS RPC builds of vlmcsd now issue a warning that protection with -o2 is poor
    - Fixed a bug that the svn version number was not included in all builds (Thx to SkyRE)
    - Fixed a bug in the MS RPC server code that ERROR_INVALID_PARAMETER instead of ERROR_INVALID_DATA was reported in some cases
    - Fixed a bug in the RPC client code that could lead in a crash if a MS RPC server reports an error in a specific way
    - vlmcsd now correctly returns an RPC error code if invalid request data was received instead of simply dropping the connection (still drops if a client sends a packet greater than a v5/v6 request to avoid DoS attacks)
    - Added SKU IDs and KMS ID for new Windows 10 SKUs, Windows Server 2016 and XC2R Editions of Visio 2016 and Project 2016
    - If random ePIDs are used, they now include build 14393 (Windows 2016 Servers) as long as you do not turn off NDR64 (-N0)
    - vlmcsd now returns an error (0x8007000D) if the minor version of the KMS protocol is not 0 to behave exactly like a real KMS server.
    - New command line option -K for vlmcs that allows to specify a KMS protocol version explicitly (e.g. 4.1) to allow requests with intentional bad data
    - Fixed a bug that vlmcsd discovered an incorrect protocol version if NDR64 was used
    - vlmcsd now reports at least 50 active clients for Windows and 10 active clients for Office to make emulator detection more difficult. It still always reports enough clients to satisfy the N count policy (minimum clients) in the request. This is just in case MS increases the N count policy of one more products
    - Specifying -t in vlmcs with an incorrect license status ( > 6 ) now works and only prints a warning (Genuine KMS servers ignore this and activate correctly)
    - Fixed a bug in vlmcs that a -r option didn't work with an argument of 1
    - New SKU IDs for rarely used products (e.g. Win 7 Enterprise E)

(**Important**: changelog between **svn777** and **977**)
    - I had to restore the changelog between svn777 and svn977 from Wayback Machine. **Unfortunately they include ad links to ebay on certain keywords** (Microsoft, Windows, Office, keyboard,. ...). I hope, I removed them all. If not, please PM me.

- 2016-07-13 (**svn977**)
    - Fixed a bug that the Port= directive in the ini file did not work if vlmcsd was compiled with full socket support (thx to mictlan).

- 2016-07-11 (**svn976**)
    - Fixed a bug with an extra colon displayed if option -T0 was used in the fork versions of vlmcsd (thx to Calistoga)

- 2016-07-10 (**svn972**)
    - Fixed some typos in vlmcsd.8
    - It is no longer possible to compile vlmcsd with deprecated options. CFLAG -DENABLE_DEPRECATED_OPTIONS has been removed.
    - New command line parameters -T0 and -T1 to allow logging without date and time on each line (requested by Calistoga)
    - New INI file parameter LogDateAndTime which does the same as -T0 and -T1
    - -o1 and -o3 now work with Android
    - New GNUmakefile command line parameter GETIFADDRS=musl to add -o1 and -o3 support for older uClibc versions that don't have getifaddrs.

- 2016-07-04 (**svn958**)
    - Bootable floppy: Removed ipcs and iprm commands from busybox because of missing kernel support.
    - Bootable floppy: busybox updated to 1.26 trunk
    - Bootable floppy: Fixed a bug that ntpd encountered an endless loop if it could not resolve the NTP server (thx to lantern)
    - New option -o1 in vlmcsd to listen on private IP addresses only
    - New option -o2 in vlmcsd to refuse clients with public IP addresses
    - New option -o3 in vlmcsd that combines -o1 and -o2
    - New compile time CFLAG -DNO_PRIVATE_IP_DETECT to compile vlmcsd without -o options.
    - New command line option for GNUmakefile NO_GETIFADDRS=1 to allow building of vlmcsd on platforms that do not support getifaddrs(). Required for older uClibc versions (autodetected for Android)
    - New INI file statement PublicIPProtectionLevel to set -o options via the config file
    - Fixed a bug in help that an incorrect warning message was displayed if vlmcsd was compiled with -DENABLE_DEPRECATED_OPTIONS

- 2016-06-17 (**svn934**)
    - Fixed a bug that -F0 and -F1 were not available in some uClibc builds due to incorrect header files in older uClibc versions.
    - Updated Intel musl toolchains to gcc 5.3.0, binutils 2.25.1 and musl 1.1.14
    - Updated armelv5 musl thumb toolchain to musl 1.1.14, gcc 6.1.0 and binutils 2.26
    - Upgraded armelv5-thumb-glibc toolchain to gcc 6.1.0, binutils 2.26 and glibc 2.23
    - Fixed a bug that vlmcsd tried to #include <semaphore.h> even if compiled with -DNO_LIMIT
    - GNUMakefile now supports building vlmcsd, vlmcs and vlmcsdmulti simultaneously (new target allmulti)
    - Fixed a bug that vlmcsd could not be compiled if NO_SOCKETS was defined without NO_LIMIT also being defined
    - Fixed a build bug that vlmcsdmulti-x86-musl was placed in the static instead of the musl directory
    - Lots of improvements for the bootable floppy disk
    - The kernel has been updated to 3.12.60
    - busybox has been updated to 1.25.git
    - busybox is now built against uClibc-ng 1.0.15 (since the original uClibc does not seem to be maintained anymore)
    - some more busybox commands (e.g. su, sulogin, nslookup, xargs, setfont, chvt, dumpkmap, cryptpw, mkpasswd). Some useless commands removed (umount, lspci)
    - Support for Latin-9 charset on local ttys
    - Support for different keyboard layouts (currently be bg br by cf cz de dk es et fi fr fr_CH gr hr hu il is it jp kg la lt lv mk nl no pl pt ro ru se sg si sk-qwertz sr trf trq ua uk us wo)
    - man page vlmcsd-floppy.7
    - Lots of options can now be configured in syslinux.cfg file (including IPv4 config, vlmcsd listening IP addresses, hostname, keyboard layout, timezone, passwords, ePIDs, HwID). See man page for details
    - If configured for DHCP, an NTP server sent by the DHCP server will now be honored
    - Upon boot there is now a 5 second timeout prompt to set IPv4 configuration and time zone manually or escape to a pre-boot shell
    - On /dev/tty8 (press ALT-F8) there is now a menu which lets you perfom certain tasks (restarting vlmcsd, turn on/off telnet and ftp, change keyboard layout / timezone, view various status information, shutdown and reboot)
    - The configuration files for building the kernel, uClibc-ng and busybox are now included

- 2016-06-05 (**svn906**)
    - Added a mips big-endian build with simple sockets to test simple sockets on a system that is big-endian and that requires data aligning
    - Added new binaries for the GNU kernel (GNU Mach with Hurd).
    - Command line option -V now displays "PIE" if the executable is position independent.
    - Command line option -V now displays "SYS R4" on operating systems that derive from Unix System V R4.
    - All pre-built binaries do no longer contain deprecated options -4, -6, -I and -f. Use CFLAG -DENABLE_DEPRECATED_OPTIONS if you need them in your own binaries.
    - If deprecated options are used in vlmcsd, it now displays a warning.
    - Fixed a bug that the supplementary group list was not replaced when using the -g option in vlmcsd (thx to FF00FF00FF)
    - Fixed a bug that the NetBSD comment was stripped in NetBSD builds (The bug is actually in NetBSD by refusing ELF binaries that do not include that comment)
    - Fixed a bug that an invalid user or group name in option -g or -u could cause an access violation.
    - Fixed a bug that errno was not saved on call of printerrorf
    - New FreeBSD binaries for FreeBSD operating systems with GNU userspace (e.g. debian GNU/kFreeBSD)
    - Option -V now displays the minimum API level in Android builds.
    - NetBSD build system was upgraded to NetBSD 7.0.1. 32-bit binaries are now build with gcc 4.8.4 (the default compiler in NetBSD 7.0.1)
    - OpenBSD build system has been updated to OpenBSD 5.9. OpenBSD binaries now build with gcc 4.9.3.
    - Fixed a bug that 32-bit NetBSD build of vlmcsdmulti crashed.
    - Ensured that all Windows binaries do not use long section names
    - Marked Windows and Cygwing 64-bit binaries compatible with ASLR
    - Added --no-seh linker flag to all Windows and Cygwin binaries except those that use Microsoft RPC
    - Added options -F0 and -F1 to allow binding to (listening on) foreign IP addresses. Only works on Linux (no restrictions) and FreeBSD (IPv4 only and requires root rights, more exactly PRIV_NETINET_BINDANY privilege). That feature can be removed by using CFLAG -DNO_FREEBIND.
    - Fixed a bug that the TCP connection backlog was set to 1 (instead of the operating system default, 128 on most OSses) if vlmcsd was compiles with simple sockets
    - Internal changes to GNUmakefile so autodetected linker flags may be directed to vlmcs or vlmcsd only. This addresses a problem that references to unneccesary libraries were included in vlmcs and vlmcsd.
    - vlmcs now respects the -DNO_VERBOSE_LOG CFLAG
    - Removed lots of code that was linked into libkms although not needed.
    - Better autodetection for required libraries so that binaries no longer reference libpthread and libresolv if not needed
    - Fixed a bug that the MaxTasks directive in the INI file could not be used if vlmcsd was compiled with MS RPC
    - GNUmakefile no longer creates a temp file if used with CAT=1 and uses a pipe instead
    - You can now build a static library libkms.a.
    - New GNUmakefile option COMPILER_LANGUAGE=c or COMPILER_LANGUAGE=c++
    - Visibility of all symbols is now set to hidden except for exported symbols in libkms.so to allow ld more efficient garbage collection.
    - GNUmakefile now uses more optimizing options by default. This causes the build process to fail with older gcc toolchains. You may use SAFE_MODE=1 on the command line to use fewer optimizations (the same as in svn855 and earlier)
    - Changed Mac OS X build script to use SAFE_MODE=1 with the older PowerPC toolchain.
    - Changed Linux build script to use SAFE_MODE=1 with several older toolchains.
    - Endianess will now be detected at runtime and is included in the output of option -V.
    - Random ePIDs no longer contain Windows 10 build number.
    - vlmcsd now assumes wrong system time if the time reported by the system is lower than the build time. This requires to build vlmcsd on a system with (somewhat) correct system time.
    - Switched linker from bfd to gold for x32 builds (smaller binaries)

- 2016-05-27 (**svn855**)
    - New compile time CFLAG -DDISABLE_DEPRECATED_OPTIONS to remove -4, -6, -I and -f from vlmcsd command line options. These are no longer needed.
    - The deprecated command line options -4, -6, -I and -f in vlmcsd no longer appear in help or the man files. However, they remain fully functional in the pre-built binaries of this release. The next version will no longer contain them.
    - New compile time CFLAG -DNO_VERSION_INFORMATION to remove the -V command line option in vlmcs and vlmcsd. Creates smaller binaries.
    - Added MSRPC=1 info to make help
    - Fixed a bug that vlmcs could not be compiled with -DNO_BASIC_PRODUCT_LIST
    - New compile time CFLAG -DSIMPLE_SOCKETS to remove -L command line option in vlmcsd. This creates smaller binaries. You can only select the TCP Port with -P. This CFLAG does not affect vlmcs. If you use an ini file, you must use the Port= and not Listen= statements
    - Pre-built libkms32.dll and libkms64.dll are now compiled with -DSIMPLE_SOCKETS
    - New API const char* const __cdecl GetEmulatorVersion() in libkms. It returns a string containing the vlmcsd version it is based on.
    - API Level of libkms has been increased to 3.1. It is fully backward compatible with 3.0.
    - The native Windows versions of vlmcsd, vlmcs and libkms are now built with mingw 4.0.4 and gcc 5.3.1 which results into smaller binaries (mingw-w64 provided with Ubuntu 16.04 LTS).

- 2016-05-21 (**svn844**)
    - Added Windows 10 Home keys to man page vlmcsd.7
    - clang can now be used as a compiler under cygwin (requires clang 3.7.1 or newer, older versions in cygwin are broken)
    - FreeBSD has been updated to 10.3 (including clang 3.8 toolchain)
    - Mac OS X/iOS: Xcode has been updated to 7.2
    - Mac OS X/iOS: gcc has been updated to 5.3.0
    - iOS: The iOS version number is no longer included in the file name in builds that use the latest Apple toolchain (use these binaries on iOS unless you experience problems)
    - Added sample build script make_lxss to build on Windows subsystem for Linux (WSL)
    - If vlmcsd encounters a failure in setting socket options IPV6_V6ONLY and SO_REUSEADDR it now reports an error if compiled with -D_PEDANTIC (this is mainly for debugging buggy Linux Kernel emulations, namely WSL)
    - Option -V in vlmcsd (display version information and exit) now reports more information including compiler version, compile time flags and target platform information.
    - Implemented a -V option in vlmcs which does more or less the same than the -V option in vlmcsd except for compile time flags that are specific for vlmcs and vlmcsd
    - Windows XP detection is now done with more generic code that also compiles under older versions of MingW32-w64.
    - Added Linux glibc binaries for SPARC 64-bit platform
    - Added Linux glibc binaries for MIPS 64-bit platform big- and little-endian with standard and micromips instruction set. The micromips instruction set has higher code density (smaller binaries) but not all MIPS 64-bit CPUs support it.
    - Updated DragonFly toolchain to gcc 5.3.0
    - Updated NetBSD 64-bit toolchain to gcc 5.3.0 (32-bit builds still use gcc 4.2)
    - Updated Solaris to 11.3 and gcc to 4.9 (5.3.0 does not work with 32-bit Solaris binaries and is marked as unstable by Oracle)

- 2016-03-07 (**svn818**)
    - Some updates to the documentation
    - Fixed a bug that could lead in wrong interpretation of boolean arguments in the INI file in rare cases
    - Fixed a memory leak bug in MS-RPC module that occured with incorrect incoming KMS requests
    - Removed iOS 4.1 builds because old compiler no longer runs on Mac OS X 10.11 (El Capitan)
    - Mac OS X gcc builds now require Mac OX X 10.11 (El Capitan) or newer because new Homebrew gcc requires this. All non-gcc build still run on Mac OS X 10.4 (Tiger) or higher.
    - OpenSSL build removed for Mac OS X because Xcode 7 does not support OpenSSL
    - Added support for IBM mainframe Linux, 31- and 64-bit (s390 and s390x)

- 2015-08-30 (**svn812**)
    - Fixed a signed/unsigned bug that caused the -m option in vlmcsd not to work under OpenBSD and NetBSD
    - Added binaries for NetBSD (Intel 32-bit and 64-bit)
    - Added binaries for OpenBSD (Intel 64-bit only)
    - Added binaries for DragonFly BSD (Intel 64-bit only)

- 2015-08-25 (**svn804**)
    - Added support for CFLAG -DINCLUDE_BETAS in the Makefile and config.h
    - Fixed a problem with obsolete -I option if vlmcsd was compiled with -D_PEDANTIC
    - Added support for ancient nameser.h in OpenBSD
    - Added makefile auto-detection that OpenBSD needs the internal DNS parser
    - FreeBSD builds now compiled with FreeBSD 10.2

- 2015-07-18 (**svn796**)
    - Updated Mac OS X toolchains to iOS8.4
    - Updated Mac OS X gcc to 5.2
    - Added KMS IDs and Activation IDs from the 10240 ADK to support Windows 10 and Office 2016. Type vlmcs -x to see all supported products
    - Updated Man page vlmcsd.8
    - Added shortcuts to -l switch (W10, W10C and O16) in vlmcs
    - Random ePID generator now includes build 10240
    - If you want Beta/Preview products to be included in the extended product list, you must now #define INCLUDE_BETAS

- 2015-06-29 (**svn785**)
    - Fixed a bug in the makefile that caused target platform detection to fail if another locale than english was used
    - Fixed a bug in the MSRPC version of vlmcs that ld erratically removed an IDL compiler generated function during optimization (thx to qewlpal)
    - Fixed a bug that too much bytes were allocated when sending a request from vlmcs

- 2015-06-23 (**svn783**)
    - Workaround for buggy MingW 4.9.2 toolchain in Cygwin that LTO can't be used
    - Updated Mac OSX gcc toolchain to gcc 5.1
    - Fixed a typo that vlmcsd svn779 displayed Office 1013 instead of Office 2013 (thx to qewlpal)
    - KMS Id list (basic product list) now says "Office 2013" instead of "Office2013" to be consistent with extended product list (SKU Id).

- 2015-05-11 (**svn779**)
    - Added GUIDs for Office 2016 preview (thx to ratzlefatz)
    - Rename KMS ID "Office2013" to just "Office" because Microsoft seems to use it for Office 2016 too

- 2015-05-01 (**svn777**)
    - FreeBSD builds now compiled with clang 3.6 and gcc 5.1
    - vlmcsd now contains a shared library (DLL on Windows, .dylib on MacOS/iOS) to allow integration in other software (e.g. LicenseManager)
    - small program libkms-test.c to test/demo a working KMS server with less than 40 lines of code
    - iOS SDK has been updated to version 8.3
    - Fixed a bug that MSRPC binaries were not linked with optimum parameters
    - Windows: marked binaries compatible with ASLR and NX
    - Windows: binaries are now terminal server aware
    - Windows: 32-bit binaries are now large address aware

- 2015-02-23 (**svn760**)
    - Windows and Cygwin versions can now use MS Crypto API for SHA256 and Hashmac calculations. Use CRYPTO=windows on the make command line
    - Added support for the buggy RPC implementation of Wine in vlmcs
    - Cleaned up KMS and RPC code a lot
    - Changed RPC memory allocation from malloc to faster stack allocation
    - New device specific binary for Asus little-endian mips routers
    - vlmcs will now detect KMS emulators that use KMSv5 IV semantics in KMSv6
    - vlmcs and vlmcsd now support the NDR64 transfer syntax and bind time feature negotiation (BTFN) in the RPC protocol. Unlike M$ vlmcsd supports NDR64 also on 32-bit platforms.
    - Changed the workaround for running vlmcsd on TCP ports < 10 to protocol compliant code
    - Fixed a bug that vlmcs could not connect to KMS servers running on TCP ports < 10
    - Fixed a bug in vlcms that caused incorrect parsing of GUIDs in options -c and -o on big-endian machines
    - Fixed a bug that 32-bit Windows/Cygwin versions using MS RPC did not work
    - Fixed an endian-related bug in vlmcs that the RPC allocation hint was byteswapped on big-endian builds
    - New options -N0 and -N1 to disable / enable the use of the NDR64 transfer syntax
    - New options -B0 and -B1 to disable / enable the use of bind time feature negotiation
    - vlmcs now detects if a KMS emulator uses XP-style RPC and activates any product that is not Office 2010 (no genuine KMS server does this)

- 2015-02-16 (**svn718**)
    - Implemented custom malloc vlmcsd_malloc which simplifies code and reduces binary size
    - vlmcs no loner uses types u_int, u_short and u_long because they are not available on every platform
    - Moved the DNS resolver code for SRV records to a seperate file (dns_srv.c)
    - Fixed a bug that vlmcs did not work, if it has been compiled with -DNO_SOCKETS
    - vlmcsd no longer logs that it has read the ini file during installation as an NT service
    - Compiled Cygwin versions with -DNO_SIGHUP because restarting vlmcsd via SIGHUP signal never worked on Cygwin
    - vlmcs and vlmcsd can now be compiled using Microsoft RPC on Windows and Cygwin. Add MSRPC=1 to your make command line
    - New ini file parameter Port = (only works if MS RPC is used)
    - New CFLAG -DSUPPORT_WINE to support running vlmcsd on wine with wine's RPC
    - New option -o in vlmcs to set the previous client machine ID
    - Added display of previous client machine ID in verbose log/display
    - Renamed KMS struct members to their official names according to https://forums.mydigitallife.info/th...l=1#post758517 (thx to deagles)
    - Changed text in help, documentation and verbose logging so it refers to the official names
    - Removed lots of gcc specific code. vlmcsd can now be compiled using ANSI C 99 / ANSI C++ 98 and higher dialects including C++ 14. No more GNU extensions required. Still conforms to ANSI C 90 with GNU extensions.
    - Fixed a bug that the 64-bit version of vlmcsd was likely to fail on XP 64-bit and Windows Server 2003 64-bit

- 2015-02-12 (**svn692**)
    - Cleaned up the code that less warnings will be displayed if vlmcsd is compiled with flag -Wextra
    - Fixed a bug that the output of vlmcs -x was directed to stderr instead of stdout
    - Better detection if the compiler supports a built-in version of alloca
    - Fixed a bug that vlmcsd could not be compiled with -DNO_LOG
    - Removed vlmcsd option -I. vlmcsd now autodetects, if it has been started from an internet superserver
    - Fixed a bug that vlmcsd logged reading the ini file on every single request if started from an internet superserver
    - Marked vlmcsd options -I, -f, -4 and -6 as deprecated in the documentation
    - vlmcsd now sets the FD_CLOEXEC flag on its sockets. That simplifies SIGHUP handling (smaller binaries).

- 2015-02-09 (**svn681**)
    - Fixed a bug that domains detected via DNS SRV were limited to 63 chars. They can now have up to 253 chars according to RFCs
    - Added an explanation in vlmcsd.8 why -r1 behaves like -r2 if vlmcsd is started in inetd mode
    - Corrected help for vlmcsd option -s in Windows and Cygwin
    - Changed some ambigous macro names
    - Fixed a bug that the ini file could eventually not be reread if vlmcsd was started via vlmcsdmulti and then restarted via SIGHUP
    - Restored the option to set a default ini file at compile time, e.g. make INI=/etc/vlmcsd.ini
    - vlmcsd now displays/logs a message if an ini file has been read
    - Fixed possible buffer overrun during DNS query in vlmcs in the native Windows version
    - Included #ifdefs in shared_globals.h to include semaphore.h and pthread.h only when they're actually required
    - GNUmakefile now correctly autodetects which libraries to include when compiling for Android
    - Added support for the Minix 3 operating system
    - Added NO_TIMEOUT=1 option to the make command line to support operating systems that do not support setting a custom timeout
    - Added CHILD_HANDLER=1 option to the make command line for operating systems that do not have a working SA_NOCLDWAIT flag to prevent "zombie processes"
    - Added proper detection in the makefile that Cygwin requires DNS_PARSER=internal

- 2015-01-30 (**svn667**)
    - Corrected and updated makefile help
    - Implemented a unified out of memory handler
    - Fixed incorrect ePIDs in config.h
    - Systems with improper system time now generate a valid ePID (thx to Nimbus2000)
    - Optimized DNS parser to use optimized compiler generated unaligned access code instead of macros with trivial code
    - Ensured that config.h is included from any *.c file
    - Fixed a bug that rand32() could return int32_t instead of uint32_t on many platforms
    - Renamed NS_GET macros so they do not conflict with resolver headers
    - Wrote more documentation of code in kms.c and rpc.c
    - Fixed a bug in Windows DNS code that vlmcs displayed wrong warnings if the DNS server return additional records (section 3 records)
    - Added the BUILD_TIME macro that contains the system time in seconds since 01/01/1970

- 2015-01-26 (**svn650**)
    - Made the output of vlmcs -G compatible with vlmcsd < svn521 (thx to mdlbrad)
    - a special file name of - (single dash character) can now be used to direct the output of vlmcs -G to stdout
    - fixed a bug that the HWID wasn't displayed correctly on XP 32-bit systems (thx to alex_voyager)
    - vlmcs can now query DNS SRV records to find KMS servers of the default or any specific domain. Doesn't currently work on Android. (thx to Wishbringer)
    - 鈥媙ew option -P in vlmcs to disable sorting of SRV records by priority and weight.
    - Added makefile option NO_DNS=1 to compile vlmcs without DNS query support.
    - Added makefile options DNS_PARSER=internal and DNS_PARSER=OS to select vlmcsd's or the OS's DNS response parser.
    - Added makefile option NOLRESOLV=1 to disable LDFLAG -lresolv (required for Android NDK toolchain)
    - Added makefile option NOLIBS=1 to include no extra libraries (currently shortcut for NOLRESOLV=1 and NOLPTHREAD=1)
    - optimized code for vlmcs -x

- 2015-01-21 (**svn614**)
    - Some code optimizations (smaller binaries of vlmcsd)
    - New switch -G in vlmcs to grab ePIDs/HWIDs from a genuine KMS server and directly update an ini file for vlmcsd.
    - Updated armv5t buildroot toolchain to glibc 2.20 / gcc 4.9.2 / binutils 2.25
    - Updated mips32 openwrt glibc toolchain to gcc 4.9.2
    - Added mips32-mips16 glibc openwert binaries
    - Replaced vlmcsd-mips16-openwrt-atheros-ar7xxx-ar9xxx-uclibc-static by a musl version
    - Added vlmcsd-mips16-openwrt-atheros-ar7xxx-ar9xxx-musl (non-static)
    - Updated x86/x64 musl toolchains to gcc 4.9.2 / binutils 2.25 / musl 1.1.6
    - On the bootable floppy vlmcsd now runs under a dedicated service account
    - The pid file on the bootable floppy has changed to /var/run/vlmcsd/vlmcsd.pid

- 2015-01-18 (**svn607**)
    - Fixed a bug that vlmcsd could not be compiled with -DNO_PID_FILE
    - Added iOS 64-bit binaries (Requires A7 or A8 CPU and iOS >= 8.0)
    - Added support for toolchains that don't have link time optimization (LTO)
    - Many binaries, especially those using older toolchains, are now smaller

- 2015-01-07 (**svn589**)
    - Added a config.h file that allows customizations when compiling vlmcsd (alternatively to a very long make command line)
    - vlmcsd can now use the ELF auxiliary vector in Linux with glibc >= 2.16 and musl. Add AUXV=1 to your make command line or config.h.
    - Better detection of target OS when compiling. Affects FreeBSD and Dragonfly only.
    - Improved SIGHUP handler for BSD operating systems.
    - Most functions are now lowercase to keep coding style checkers happy.
    - changed some boolean values from 32-bit to 8-bit (smaller binary)
    - Makefile now let you create dependency files (*.d) to better support rebuilding vlmcsd after a change of a *.h file. Add DEPENDENCIES=1 to your make command line.
    - Moved lots of #include directives from *.h to *.c files to avoid unnecessary recompile (vlmcsd compiles faster)
    - Updated Fritzbox big-endian toolchain to OS version 6.20 (binaries still run on OS 5.x and Freetz)
    - New make command line option VERBOSE=3 that causes the compiler name to be displayed during builds (useful if you batch compile for many platforms)
    - Fixed an alignment bug that caused vlmcsd and vlmcs to crash when they displayed/logged a GUID. Affected little-endian mips builds that used the compressed instruction set mips16 only and is actually a gcc bug.

- 2015-01-02 (**svn560**)
    - Added lots of configuration items to the ini file
    - Added sample ini file (vlmcsd.ini)
    - Changed the SIGHUP to a much simpler implementation
    - Fixed a bug that could cause a program crash if a malformed GUID was used in the ini file
    - Fixed GNUmakefile so that it no longer compiles ntservice.c if the target is not Windows or Cygwin
    - Started to change all functions to lowercase to keep certain style checkers happy (affects source code only)
    - Removed some unnecessary global variables (smaller binaries)
    - Fixed a bug that could cause a crash or malformed output if an incorrect user or group name was specified with -u or -g
    - Implemented more specific error messages for -u and -g
    - Added a seperate man page for the ini file (vlmcsd.ini.5)
    - Added new makefile option NOPROCFS=1 that allows to build vlmcsd without a dependency to a properly mounted proc filesystem in /proc and made this the default for FreeBSD.
    - Added new CFLAG "-DNO_SIGHUP" to compile vlmcsd without a handler for SIGHUP.
    - Changed vlmcsd invocation on the bootable floppy so it uses the ini file instead of command line parameters whenever possible.

- 2014-12-19 (**svn538**)
    - Fixed a bug in vlmcsd.c that could cause compiler warnings about unused functions
    - changed Android toolchain from Ubuntu 14.10 to Google NDK 10d
    - re-included non-PIEs for Android (Android 1.5 to 4.4)
    - Android PIEs (Android 4.1 and up) no longer issue a warning about text relocations
    - Improved detection whether shared memory functions require direct usage of syscall
    - Added Makefile parameter NOLPTHREAD=1 to disable automatic detection whether linker needs to link against pthread library (required for Google NDK)
    - Added support for Android devices with x86 CPU
    - Added support for Android devices with Mips CPU (I'm not aware of any devices)
    - Added support for 64-bit Android (Intel, Mips and Arm)
    - Changed armv5 builds to armv5te since Android does not run on any CPU < armv5te resulting in smaller binaries for very old Android phones
    - Updated FreeBSD binaries to FreeBSD 10.1 build system (gcc 4.9 and clang 3.5)
    - Updated Mac OS X binaries to Mac OS X 10.10 (Yosemite) build system with gcc4.9 and clang 3.5

- 2014-12-08 (**svn531**)
    - Fixed a bug in vlmcs, that caused a false error display if vlmcs was run against certain emulators under XP
    - Added support for Windows 10 Core Preview
    - switched iPhone binaries from 8.0 SDK to 8.1 SDK
    - Android binaries are now position independent (thx to pantagruel)

- 2014-10-03 (**svn527**)
    - Added support for Windows 10 Preview Professional WMC
    - Added support for Windows 8.1 Core Connected incl. N, SL, CS, marketing name Windows 8.1 with Bing (thanks to Carlos Detweiller and abbodi1406)
    - Added support for Windows 8.1 Professional Student incl. N (thanks to Carlos Detweiller and abbodi1406)

- 2014-10-01 (**svn525**)
    - Added support for Windows 10 Preview (Professional and Enterprise)
    - Added support for Windows Server 2015??? Preview (Datacenter)

- 2014-09-30 (**svn524**)
    - Fixed a bug (appeared in svn498) in Windows and Cygwin versions that vlmcsd did not exit cleanly if it was started as a service
    - Updated man page vlmcsd.8 to reflect changes in svn521

- 2014-09-29 (**svn521**)
    - Added buildroot configuration files to allow easy creation of cross-compile toolchains
    - vlmcsd now reads the ini file at program startup and no longer with each activation
    - Added SIGHUP handler to reread the ini file without restarting the program
    - Changed internal GUID representation that it is always host-endian except in the actual KMS request and response where it is little-endian. This reduces number of endian conversions.
    - New unified GUID parser for vlmcsd and vlmcs
    - Simpler conversion from binary GUID to string GUID
    - Fixed a bug for type ProdListIndex_t which is now uint8_t across all toolchains and platforms
    - Fixed a bug in the ppc uclibc build that it didn't work with older kernels (thx to pantagruel)

- 2014-09-21 (**svn498**)
    - Removed vlmcsd options -I, -u and -g from native Windows builds (remain in Cygwin)
    - Fixed a bug in vlmcsd socket error handling that could cause program termination in Solaris
    - Fixed a bug in vlmcsd socket error handling that could cause a segmentation fault in Darwin (Mac OS / iOS) if the client disconnects in the middle of a KMS request
    - Fixed a bug in vlmcs that caused it not reestablish an RPC session that got interrupted
    - Fixed a typo in vlmcs -x
    - Fixed a bug in vlmcs that it displayed wrong a product list if vlmcs was compiled with -DNO_BASIC_PRODUCT_LIST
    - The fork() version of vlmcsd now logs a warning if a forked process crashes or gets killed
    - New option -m in vlmcsd that sets a maximum of forked processes / threads
    - New option -t in vlmcsd that specifies a custom RPC session timeout
    - New option -d that disconnects clients after each KMS response
    - New options -i4 and -i6 in vlmcs that allow to explicitly select IPv4 or IPv6
    - New option -p in vlmcs that forces it to use non-multiplexed RPC
    - New binaries for iOS 4.1, 5.1, 6.1 and 8.0 plus armv6 binaries for iOS
    - The bootable floppy now starts vlmcsd under an unprivileged guest account
    - Fixed a bug in the bootable floppy that /dev/null was not writeable for all users
    - Updated GNUmakefile that automatically detects if linking against libpthread is required
    - Changed Cygwin version from Posix threads to Windows threads because the pthreads implementation of Cygwin is buggy (memory leak)
    - New compile time option -DNO_LIMIT to compile vlmcsd without support for -m

- 2014-09-15 (**svn448**)
    - -l option in vlmcs let's you select every product that vlmcsd is aware of
    - -l now also accepts a number instead of a product name in vlmcs. Type vlmcs -x to list available products
    - added binaries for PowerPC based Macintoshes
    - added binaries for Sparc CPUs (Linux only)
    - added binaries for older versions of iOS
    - fixed a signed/unsigned bug in vlmcsd
    - added binaries for 64-bit PowerPC (Linux only)
    - fixed a bug in vlmcsd that caused a crash if the client sent an unknown application id
    - converted the string GUIDs in vlmcs to binary GUIDs (smaller binary)
    - optimized help output in vlmcsd
    - vlmcs now also accepts the NO_HELP CFLAG (like vlmcsd does)
    - vlmcsdmulti can now also be called directly without creating symbolic links
    - fixed a bug in vlmcs that an out of memory condition was not handled correctly
    - Reworked the code that it compiles with compilers that do not use the C90 dialect as the default dialect, e.g. Apple llvm-gcc42
    - the bootable floppy now has a new busybox with less command but more features, e.g. help
    - the bootable floppy now has an ftp server turned on by default
    - Better checking of numeric arguments in vlmcsd

- 2014-09-07 (**svn424**)
    - New parameter -C that allows to specify a locale id (LCID) if the ePID is random
    - added dynamically linked musl binaries
    - Added support for building a multi-call binary (vlmcs and vlmcsd in a single binary)
    - Linux build script reworked to be more readable
    - Fixed a bug in error reporting that caused stdout instead of stderr to be flushed in some cases
    - Corrected a bug in RPC handling that caused vlmcs to display a false warning about the RPC_PF_MULTIPLEX flag in some cases
    - Changed RPC_PF_MULTIPLEX handling in vlmcsd to match behavior of M$ RPC
    - armv5 binaries have been renamed so you can see whether they use thumb instruction set or not

- 2014-08-16 (**svn400**)
    - vlmcs and vlmcsd can be compiled with g++ under Windows
    - vlmcs compiles to smaller binaries if compiled with older versions of gcc / ld.bfd which behave poor while removing unused code and data
    - more detailed error messages in setup of the listening sockets
    - Fixed a bug in error display when you request IPv6 on an IPv4-only machine
    - Fixed a bug that prevented vlmcsd to send an activation response if the client is XP 32-bit (thanks to mu70)
    - If you compile vlmcsd with -D_PEDANTIC it performs a thorough check of the RPC message and displays detailed error messages. Compiling with -D_PEDANTIC adds about 3 kB to your binary
    - Fixed a bug that caused false reports about incorrect GUIDs (ocurred only on big-endian builds if compiled with -D_PEDANTIC)

- 2014-08-13 (**svn390**)
    - Fixed a bug that caused compilation to fail if NO_LOG was defined
    - Some more work on OpenSSL hacking to allow compiling working vlmcs binaries with AES via OpenSSL.
    - Included some vlmcs binaries with OpenSSL support
    - All OpenSSL binaries now clearly marked as experimental
    - Included file README.openssl to explain why using OpenSSL with vlmcs/vlmcsd fails in many cases
    - Included many new binaries that I compiled with newly created OpenWRT toolchains. Especially MIPS binaries are up 7 kB smaller compared to corresponding buildroot toolchains.
    - Created new handcrafted PPC toolchain that produces much smaller binaries
    - Renamed Makefile to GNUmakefile and created a new Makefile that acts as wrapper on systems where GNU make is not the default make (FreeBSD, Solaris).
    - Added README.compile-and-pre-built-binaries which briefly explains how to compile vlmcsd or select a proper binary
    - Completely restructed directory hierachy for the binaries. Binaries are now in binaries/<os>/<cpu>/<endianess>/<C library>. This should make it easier to find a binary that runs on a specific system or device.


- 2014-08-11 (**svn371**)
    - Changed Makefile so that vlmcsd and vlmcs display date and time in version information in a common format across all platforms
    - Changed hash style of ELF binaries from sysv to gnu where possible (all Intel and ARM binaries except Android and Solaris) to reduce binary file size
    - Solaris binaries now built with gcc 4.8 on Solaris 11.2. Binaries do run on Solaris 11.1 and recent OpenIndiana versions
    - Added static binary for OpenWRT with AR7xxx/9xxx MIPS CPUs
    - FreeBSD binaries now include gcc builds (smaller binaries)
    - Renamed byte swap macros to BS16, BS32 and BS64 to avoid conflicts with Linux SDK names. Reduces binary file size for compiler versions that only support some byte swap built-ins, e.g. GCC < 4.8
    - Added a workaround for clang that is unable to process asm statements found in the Windows SDK header files of MingW32-W64 and cygwin.
    - Removed packed attribute on structs where they weren't neccessary. Greatly reduces code size on CPUs with proper alignment requirement (MIPS, ARM, PPC)
    - Fixed wrong filenames for little endian MIPS OpenWRT builds
    - Changed all parts of the code that were incompatible with C++. vlmcsd can now be compiled with either a C++ compiler or a C compiler

- 2014-08-04 (**svn355**)
    - Corrected a small bug in vlmcsd help text
    - Corrected Client SKU Id for Visio Standard 2013 (thanks to ratzlefatz)
    - Fixed a bug with clang not using built-in functions for byte swapping
    - Added new binaries for Mac OS X built with gcc 4.9 which are somewhat smaller than those built with clang 3.4

- 2014-07-17 (**svn349**)
    - Secured vlmcsd against possible malicious KMS requests that may have caused a crash if verbose logging was used
    - If vlmcsd is compiled with -D_PEDANTIC it performs some additional sanity checks on the KMS request
    - New switches -l W8C and -l W81C in vlmcs to simulate Windows 8/8.1 Core
    - Fixed a bug in command line parsing of vlmcs that could lead to ignoring -a, -s, -k and -r switches in very rare cases

- 2014-07-15 (**svn344**)
    - vlmcs now generates client UUIDs 100% RFC4122 compliant to version 4 UUIDs
    - Fixed Linux build script that caused some binaries missing in svn339
    - Fixed FreeBSD build script (wrong filename in x86 binary)

- 2014-07-14 (**svn339**)
    - corrected client GUID generation in vlmcs so that the first digit of the 3rd group is always 4
    - vlmcs now displays the product name of the Application GUID, SKU GUID and KMS GUID in verbose mode
    - Implemented verbose logging in vlmcsd. New switch -v
    - The bootable floppy now uses ntpd to get the system time which is more reliable than rdate
    - The bootable floppy now uses verbose logging by default
    - new CFLAG -DNO_VERBOSE_LOG to disable verbose logging at compile time

- 2014-07-02 (**svn336**)
    - Fixed a bug that vlmcsd was logging a shutdown message when in inetd mode
    - Corrected man page vlmcsd.7 that it correctly states 30 or 45 days for KMS activation
    - New binary for openwrt routers based on Atheros AR 7xxx/9xxx chips
    - Improved logging in threaded binaries of vlmcsd (better thread synchronization, bigger message buffer)
    - vlmcsd now correctly handles errors when -L option is used more often than the default FD_SETSIZE permits (64 on Windows, 1024 on most Unixes)
    - Fixed a bug in the Solaris build script so that it creates proper 32 bit Solaris binaries now. All Solaris binaries also run under OpenIndiana.
    - Added 32 bit binaries for FreeBSD
    - Changed endian.h to use built-in compiler functions for byteswapping (supported by gcc and clang).

- 2014-06-18 (**svn315**)
    - vlmcsd now contains man pages (also available as PDF, HTML and plain text files)
    - Fixed a bug in -e option of vlmcs (wrong example displayed)
    - Better parsing in hostname and ip address parsing (may now always be included in brackets on all platforms)
    - On program termination with SIGTERM (standard Unix kill) and SIGINT (Ctrl-C when in foreground mode) vlmcsd now logs a termination message and deletes the pid file
    - The threaded version now uses a mutex (Unix) or a critical section (Windows) to ensure proper logging even with many simultanous clients.
    - Fixed a bug in the native Windows version that vlmcsd did not report an error if another process was already listening on the tcp port(s).

- 2014-06-09 (**svn270**)
    - New options -w, -0 and -3 to specify ePIDs for Windows, Office 2010 and Office 2013 from command line
    - New option -H to specify HwId from command line
    - New CFLAG -DNO_CL_PIDS to compile vlmcsd without support for -H, -w, -0 and -3
    - New binary for tomato little endian mips. The static tomato binary does not support IPv6.
    - New binary for openwrt little endian mips.
    - vlmcs and vlmcsd can now be compiled for Solaris (binaries included)
    - Smaller binaries for Linux and FreeBSD (removing unneeded sections from LD output and run sstrip to remove section headers)

- 2014-06-06 (**svn252**)
    - improved display output in vlmcs (client)
    - changed default host in vlmcs to 127.0.0.1 because 127.0.0.2 does not map to localhost in BSD descendants (e.g. FreeBSD, MacOS)
    - changed RPC protocol checking in vlmcs in way that it better handles emulators which are not 100 % compliant to DCE-RPC
    - vlmcs now works with KMS emulators that disconnect the client after each activation response
    - new option -T in vlmcs that causes to re-establish the RPC connection after each request. Useful for memory leak detection.
    - fixed a bug in vlmcs that could lead to an erroneous report of a non-zero NDR32 result code
    - better error messages when vlmcs talks to a real KMS server

- 2014-06-05 (**svn241**)
    - vlmcsd now contains a KMS client/charger called vlmcs.
    - Fixed a bug when installing the Windows or Cygwin version as a service
    - Fixed a bug with incorrect logging when ePID comes from an INI file
    - Fixed a bug in the random generator when RAND_MAX is 0x7fff
    - Fixed some long/int conversion bugs (affected MingW/Cygwin only)
    - vlmcsd can now be compiled with "-flto=jobserver" (gcc only) to allow parallel link time optimization (faster compile)

- 2014-05-27 (**svn189**)
    - Native Windows version is no longer experimental and now nearly fully featured.
    - Added the ability to compile for pthreads and Windows threads in addition to fork() (vityan666)
    - Fixed a bug in the native Windows version that resulted in a socket timeout of 60 ms which made it a LAN-only version
    - In addition to the native Windows version the Cygwin builds can be installed as an NT service
    - Added the ability to remove an NT service using -S (vityan666)
    - Fixed a bug in the Windows version that caused socket ressources not to be released (memory leak)
    - Added the ability to specify user (-U) and password (-W) credentials under which the NT service should run.
    - Implemented a workaround for a bug in RPC code when using a TCP port < 10 (Full fix coming soon)
    - You can now compile custom default ePIDs/HWID (e.g. which you grabbed from a real KMS server) via the make command line. Eliminates the need for an ini file.

- 2014-05-23 (**svn170**)
    - Fixed a bug in endian.h that prevented correct compile with -O0 and -Og on cygwin and mingw
    - More binaries for mips and arm
    - Many static builds now built with musl instead of uClibc (smaller binaries)
    - Added an x32 binary (requires 64-bit Linux kernel 3.4 or newer, glibc 2.16 or newer)
    - glibc builds no longer require glibc newer than 2.4 (except x32)
    - kms.c has been restructed to be more readable (more and smaller functions)
    - workaround for gcc 4.8 memset bug (incorrect warning)
    - vlmcsd now compiles warning free with -Wall which is now the default
    - WCHAR is now unsigned int16 across all platforms
    - moved some stuff from main.c to network.c, output.c, nix.c and shared_globals.c (vityan666)
    - native Windows version can now run as an NT service (vityan666). Cygwin to come.

- 2014-05-10 (**svn136**)
    - vlmcsd can now be started via an internet superserver, e.g. inetd or xinetd) using -I
    - Total rework of the Makefile allowing parallel (multi CPU use) builds (using make -j)
    - New binaries for Android devices with armv7 (or newer) CPUs (smaller binaries than armv6)
    - Fixed a bug in the INI-File parser. All UTF-8 chars are now recognized correctly.
    - Added some dirty hacks on openssl that allow it to perform KMSv6 modified AES encryption on most platforms and Rijndael-160 CMAC (as used in KMSv4 hashing) on some platforms.
    - Added support for openssl flavors that have no support for HMAC (as on many embedded devices, especially routers)
    - Removed the KMS stuff from rpc.c and put it in seperate file kms.c
    - Lots of optimizations in the crypto stuff for smaller binaries
    - Removed nasty warnings when building MacOS/iOS versions
    - Fixed an endian related bug with HWIDs
    - Removed all unneccassary unaligned access macros
    - Big endian Fritzbox binaries (73xx/74xx/63xx/33xx) now make use of MIPS16 ASE (smaller binaries)
    - Pre-compiled binaries are now built with LTO (link time optimization) if the toolchain supports it

- 2014-04-15 (**svn86**)
    - Fixed a bug with incorrect ePIDs if no AppGUID was found in vlmcsd.ini
    - You can type make help to see some options for compiling vlmcsd
    - iOS binary now built with iOS 7.1 SDK
    - Mac OS X binaries now run on 10.0 and higher (was 10.4 and higher before)
    - Reworked Makefile so you can type make (without options) on Mac OS X machines
    - Added x64 binary for FreeBSD. Tested with 9.1 and 10.1
    - OpenSSL now working with KMS V6 (fixed a bug that HMAC_CTX_init was not called before HMAC_Init_ex and thus causing a core dump)
    - You can now disable features at compile time to produce smaller binaries
    - New options -A and -R to send custom activation retry and renewal intervals to the client
    - Binaries for big endian Fritzboxes now built with FritzOS 6.xx toolchain
    - Added some binaries that are linked against OpenSSL
    - vlmcsd now has a version identifier. Use vlmcsd -V to display it. This release is svn86.
    - Bootable floppy now tries to get the correct time and date from time.nist.gov

- 2014-01-25
    - added support for Hardware IDs (HWIDs)
    - new option -e to log to stdout but do not run in foreground
    - new option -D to run in foreground but log according to -l option (useful for some advanced sysv-init replacements like launchd, upstart or systemd. Also useful for cygrunsrv)
    - option -f works the same as in previous versions and combines -D and -e
    - the bootable floppy now reads /etc/vlmcsd.ini which you can edit to change HWIDs

- 2014-01-21 & 2014-01-18
    - new options -u and -g allow running vlmcsd under different (less privileged) user and group
    - option -l syslog causes all logging output to be written to syslog
    - -L statement now accepts an optional scope id and port, e.g. -L 192.168.1.1:1688 or -L [fe80::1234:56ff:fe78:9abc%eth0]:1688
    - ports and scope ids can now be either an integer number or a name if your platform supports it
    - new binary specifically for the Raspberry PI using hard-float instructions
    - new binary for big-endian ARM systems
    - all static binaries now linked against 碌Clibc which makes them smaller
    - fixed a bug in command line parsing
    - updated README
    - Added some more KMS and SKU IDs (e.g. Win 8/8.1 Core Single Language)
    - All messages to stderr now clearly show "Warning" (vlmcsd can continue) or "Fatal" (vlmcsd has been terminated)
    - IPv6 mapped IPv4 addresses are no longer used. You can now use netstat or lsof -i to determine whether vlmcsd listens to IPv4, IPv6 or both.
    - It is now possible to listen to specific IP addresses only (Complete rewrite of socket handling).
    - Unlimited combinations of IP addresses and ports
    - Supports link local IPv6 addresses with scope IDs
    - README file is not yet updated because I plan further enhancements. Please see here.


- 2013-12-17
    - RPC secondary address (TCP Port) now correctly included in RPC header (crony12)
    - no more huge pre-computed V4 hash table. Saves 24 kB file size (crony12)
    - vlmcsd logs more details on the product, e.g. "Office 2013 Professional Plus" instead of "Office 2013".


- 2013-12-06
    - fixed a bug with incorrect calling conventions on 32-bit Windows (Thx to eIcn)
    - fixed a bug with non-zero RPC return codes on Windows and Cygwin (Thx to qad and eIcn)
    - vlmcsd no longer uses default ini and pid files. You must use -i and -p explicitly
    - random ePIDs are now supported by using the new -r option
    - now includes a README file
    - the bootable floppy (for virtual machines) is now part of the binary distribution

- 2013-11-27
    - Corrected a bug in RPC handling that could lead to wrong packet size in rare cases
    - vlmcsd now shows/logs the product that is being activated (KMS v6.0 request from pop3.microsoft.it for Windows 8.1)
    - some changes to the source code so it compiles under cygwin (cygwin version on WinXP does not support IPv6, needs Vista or higher)
    - experimental WIN32 version added (no cygwin required). The x86 file is guaranteed to fail. The x64 version may work on Windows >= Vista.

- 2013-11-23
    - Some minor changes to allow compiling vlmcsd for Mac OS X and iOS

- 2013-11-22
    - IPv6 support
    - Fixed a bug with incorrect RPC buffer size when using V5 activation on BE (big endian) systems
    - Refactored CreateResponseBase that is does not break Eclipse's syntax coloring so badly
    - Implemented proper UCS-2LE to UTF-8 conversion routines (vice versa needs to be done)
    - Merged DougQaid's logging and reworked it a little bit
    - Workstation name (from Request) in logs
    - New command line parameter -f that causes foreground operation and logging to stdout


