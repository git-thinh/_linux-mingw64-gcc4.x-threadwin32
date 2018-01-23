////////////////////////////////////////////////////////////////////////////////////////////
	MINGW64: x86_64 GCC-4.9.0 BUILD & INSTALL C++ ON LINUX [THREAD MODE: WIN32]
////////////////////////////////////////////////////////////////////////////////////////////

	#Go to SU to install the 1.0 cross-compilers for all users
		$ su        #SU to root; give root password to continue
		$ cd /usr/  #Make installation directories in /usr/
		$ mkdir mw32   #If this is the first time, and mw32 doesn't exist yet
		$ mkdir mw64   #If this is the first time, and mw64 doesn't exist yet

		#Install the 1.0 W32 cross-compiler
		$ cd mw32
		$ rm -rf ./*    #Remove previous installation, if any
		$ tar -xvf mingw-w64-bin_x86_64-linux_20131228.tar.bz2
		$ cd ..

	#Install the W64 cross-compiler
		$ cd mw64
		$ rm -rf ./*    #Remove previous installation, if any
		$ tar -xvf mingw-w64-bin_x86_64-linux_20131228.tar.bz2

		$ ./x86_64-w64-mingw32-gcc main.cpp -o _main.exe
		$ ./x86_64-w64-mingw32-gcc -v
		$ ./x86_64-w64-mingw32-gcc --version

	# Build & install OpenSSL:
		$ tar zxvf openssl-1.0.0e.tar.gz
		$ cd openssl-1.0.0e
		$ CROSS_COMPILE="x86_64-w64-mingw32-" ./Configure mingw64 no-asm shared --prefix=/usr/mw64
		$ PATH=$PATH:/usr/mw64/bin make
		$ PATH=$PATH:/usr/mw64/bin make install

	# Build & install Protobuf:
		$ sudo apt-get install autoconf automake libtool curl make g++ unzip		
		$ git clone https://github.com/google/protobuf.git && cd grpc		
		$ git submodule update --init
		
		$ ./autogen.sh
		
		$ mkdir configure && cd configure
		$ ./configure
		
		$ CROSS_COMPILE="x86_64-w64-mingw32-" ./configure mingw64 no-asm shared --prefix=/usr/mw64
		$ PATH=$PATH:/usr/mw64/bin make
		$ PATH=$PATH:/usr/mw64/bin make install
		
		$ make check
		$ sudo make install
		$ sudo ldconfig # refresh shared library cache.	
		
				
	# Build & install GRPC:
		> @rem You can also do just "git clone --recursive -b THE_BRANCH_YOU_WANT https://github.com/grpc/grpc"
		> powershell git clone --recursive -b ((New-Object System.Net.WebClient).DownloadString(\"https://grpc.io/release\").Trim()) https://github.com/grpc/grpc
		> cd grpc
		> @rem To update submodules at later time, run "git submodule update --init"
		
		$ sudo apt-get install autoconf automake libtool curl make g++ unzip		
		$ git clone https://github.com/grpc/grpc.git && cd grpc
		$ git submodule update --init
		
		$ mkdir configure && cd configure
		$ ./configure
		
		$ CROSS_COMPILE="x86_64-w64-mingw32-" ./configure mingw64 no-asm shared --prefix=/usr/mw64
		$ PATH=$PATH:/usr/mw64/bin make
		$ PATH=$PATH:/usr/mw64/bin make install
	
	
	
	
	$ export PATH=$PATH:/mnt/c/mingw-w64-x86_64-gcc7.2.0-win32-sjlj/mingw64/bin
	$ export PATH=/mnt/c/mingw-w64-x86_64-gcc7.2.0-win32-sjlj/mingw64/bin:$PATH
	$ export PATH=$PATH:/c/Program\ Files/GTK+-2.22/bin

////////////////////////////////////////////////////////////////////////////////////////////
UsingLinuxBinaries
////////////////////////////////////////////////////////////////////////////////////////////

Overview
Here we will discuss installation of mingw-w64 cross-compilers for Windows 32-bit and 64-bit cross-compilers under Linux. There are two types of binaries for mingw-w64 on-line right now, the "1.0" stable and snapshot builds, and the "cutting edge" builds that use the latest available builds of gcc and other compilers. They are installed differently (for now) but are used the same way. Both run under Linux and generate Windows binary executable files of the format foo.exe. If you do your own builds from the source code, one method or the other, or both, should work for you, with minor changes that reflect the way that you do things.

The examples given here are bash scripts for Linux x86_64 that have been tested under Fedora Core 14. Differences for use under 32-bit Linux or other distributions and even different shells are left as an exercise for the reader. You need to edit these script to make the build dates match those of your downloaded tarballs, or you can use more elaborate scripts.

Installing
Installing the 1.0 Stable Releases and Daily Builds
The daily builds are available here. The binary files are of the form

mingw-w64-1.0-bin-x86_64-linux-[yyyymmdd].tar.bz2 for generating Windows 64-bit binaries, and mingw-w32-1.0-bin-x86_64-linux-[yyyymmdd].tar.bz2 for generating Windows 32-bit binaries,

where [yyyymmdd] is the build date, as 20110222 for February 22, 2011.

To install for all users, su to root and install in /usr/mw32 and /usr/mw64; you may select other folder names than mw32 and mw64. A bash script that will accomplish this is

  #Go to SU to install the 1.0 cross-compilers for all users
  su        #SU to root; give root password to continue
  cd /usr/  #Make installation directories in /usr/
  #mkdir mw32   #If this is the first time, and mw32 doesn't exist yet
  #mkdir mw64   #If this is the first time, and mw64 doesn't exist yet

  #Install the 1.0 W32 cross-compiler
  cd mw32
  rm -rf ./*    #Remove previous installation, if any
  tar -xvf /home/beard9g/Downloads/mingw-w32-1.0-bin_x86_64-linux_[yyyymmdd].tar.bz2
  cd ..

  #Install the W64 cross-compiler
  cd mw64
  rm -rf ./*    #Remove previous installation, if any
  tar -xvf /home/beard9g/Downloads/mingw-w64-1.0-bin_x86_64-linux_[yyyymmdd].tar.bz2
Installing the bleeding edge builds
The bleeding edge builds will not install under root and execute by users unless you su to root; you will get permission errors. Until this is changed, you need to install these builds locally. A bash script that will accomplish this is

  #INSTALLATION SCRIPT FOR BLEEDING EDGE COMPILERS
  #Install bleeding edge compilers locally, in ~
  #mkdir mw32   #If this is the first time, and mw32 doesn't exist yet
  #mkdir mw64   #If this is the first time, and mw64 doesn't exist yet

  #Install the W32 cross-compiler
  cd mw32
  rm -rf ./*    #Remove previous installation, if any
  tar -xvf ../Downloads/mingw-w32-bin_x86_64-linux_20110219.tar.bz2
  cd ..

  #Install the W64 cross-compiler
  cd mw64
  rm -rf ./*    #Remove previous installation, if any
  tar -xvf ../Downloads/mingw-w64-bin_x86_64-linux_20110214.tar.bz2
Using the cross-compilers to generate Windows 32-bit and 64-bit binary executables
All you need to do to use any of the compilers is to insert mw32/bin or mw64/bin into the PATH at the beginning. The compilers that generate Windows 32-bit scripts use a prefix of i686-w64-mingw32- (or, -w32- for Linux x86) on the usual Linux compiler binary file names, the compilers that generate the Windows 64-bit scripts use a prefix of x86_64-w64-mingw32- (or -w32-). The host options m32 and m64 are used in the example below for clarity and emphasis. Note that if no cross-compiler is used, the script reverts to a simple native compilation as installed in your Linux distribution, which may be useful for development and testing. The example below is cut down from a project that compiled a C engine that was executed as part of a package that used a Fortran 95/2003 main program. The calls to echo the version and target architectures are for keeping track of which versions are used in the builds and may be commented out if you don't want that information every build.

  #CROSS-COMPILATION SCRIPT
  #Pick just one, and only one, compiler to use; pick none to use system gcc
  CCW32=0
  CCW64=0
  CCW32BE=0
  CCW64BE=1

  if [ $CCW32 = 1 ]; then
  CCPATH="~/mw32/bin/:"
  PREFIX="i686-w64-mingw32-"
  SIZEOPT="-m32"

  elif [ $CCW64=1 ]; then
  CCPATH="~/mw64/bin/:"
  PREFIX="x86_64-w64-mingw32-"
  SIZEOPT="-m64"

  elif [ $CCW32BE = 1 ]; then
  CCPATH="/usr/mw32/bin/:"
  PREFIX="i686-w64-mingw32-"
  SIZEOPT="-m32"

  elif [ $CCW64BE = 1 ]; then
  CCPATH="/usr/mw64/bin/:
  PREFIX="x86_64-w64-mingw32-"
  SIZEOPT="-m64"

  else
  CCPATH=""
  PREFIX=""
  SIZEOPT=""
  fi

  #Add path to cross-compiler at beginning of PATH
  PATH=$CCPATH$PATH #Works for all compiler calls within the shell of this script
  #export PATH  #Will not take unless you are root but may be useful if you call other scripts from this one

  #Get information on versions
  $PREFIX"gcc" --version
  $PREFIX"gcc" -dumpmachine
  $PREFIX"gfortran" --version
  $PREFIX"gfortran" -dumpmachine

  #Use the compiler
  $PREFIX"gcc" [options] -static $SIZEOPT -c [your C or C++ program, compiled to .o]
  $PREFIX"gfortran" [options] -static [Fortran 95 programs and modules] $SIZEOPT -o [your executable binary name].exe

  #PATH will revert to the system value when the script exits


////////////////////////////////////////////////////////////////////////////////////////////
MinGW-w64 personal 'sezero' build from 2011-11-01.
////////////////////////////////////////////////////////////////////////////////////////////

Source tarball:
----------------
	See the mingw-w64-src_20111101_sezero.tar.gz
	file under Toolchain sources -> Personal Builds

Included software versions:

Common in both cross- and native-toolchains:

- gcc: 4.5.4-prerelease (svn r.180676 with patches)
- binutils: 2.21.90 (cvs / 2.22 branch, 2011-10-31 10:30
  GMT) with patches.
- mingw-w64-crt: v1.0.1 (svn revision 4560 / 2011-10-29)
- mingw-w64-headers: v1.0.1 (svn rev. 4537 / 2011-10-11)
  with a minor patch
- glext headers: 2011-10-03 (from the Khronos Group)
- opencl headers: 2010-08-19 (from the Khronos Group)
- pthreads-win32: 2.9.0 (CVS 2010-02-28 20:00 GMT) with
  w64 patch applied.

In native-toolchains only:

- gmp : 4.3.2 (with w64 patch applied)
- mpfr: 2.4.2-p3
- mpc : 0.8.2
- gdb : 7.3.50 (CVS 2011-10-31 / 14:30 GMT, with minor
  w64 patches applied)
- make: 3.82   (CVS 2010-08-27 / 16:20 GMT)
- gendef, libmangle: from mingw-w64 svn/trunk (revision
  3572, 2010-09-14)

Files	:
----------------
mingw-w64-bin_i686-linux_20111101_sezero.tar.gz
	This is a cross compiler toolchain for
	running on a i686-linux host and creating
	x64-windows binaries.

mingw-w64-bin_x86_64-linux_20111101_sezero.tar.gz
	This is a cross compiler toolchain for
	running on a x86_64-linux host and creating
	x64-windows binaries.

mingw-w64-bin_x86_64-mingw_20111101_sezero.zip
	This is a native compiler toolchain for
	running on x64-windows host and creating
	x64-windows binaries.

mingw-w64-bin_i686-mingw_20111101_sezero.zip
	This is a cross compiler toolchain for
	running on x86-windows host but creating
	x64-windows binaries.

ChangeLog	:
-----------------

Build 2011-11-01.
--------------------------------------------------------
- GCC updated to 4.5.4-prerelease (svn rev. 180676) in
  order to catch up with some upstream fixes.
- Added the SUBTARGET_FRAME_POINTER_REQUIRED fix from
  gcc-4.6.2 rev.180422.
- Updated mingw-w64 crt to rev.4560/2011-10-29.
- GDB updated to CVS version 7.3.50 2011-10-31/14:30 GMT

Build 2011-10-27.
--------------------------------------------------------
- GCC updated to 4.5.4-prerelease (svn rev. 180340) in
  order to catch up with some upstream fixes.
- Added fixes for PR/28047 and DW2 signal unwinder from
  gcc-4.6.
- Binutils updated to CVS version 2011-10-27/12:20 GMT
  (binutils-2_22-branch) in order to catch up with some
  upstream fixes.
- GDB updated to CVS version 7.3.50 2011-10-27/12:20 GMT
  in order to catch up with some upstream fixes.
- Updated mingw-w64 crt and headers to v1.0.1 release
  (svn rev. 4537/2011-10-11)
- Updated OpenGL extension headers from Khronos Group to
  Oct. 03, 2011.
- Updated gmp to 5.0.2 release for win-native builds.

Build 2010-10-02.
--------------------------------------------------------
- Initial build based on gcc-4.5.2.


Compatibility Notice:  *** No leading underscore ***
--------------------------------------------------------
Unlike the other builds from mingw-w64 up to 2010-04-27,
these new Win64 targetting toolchains do *not* prepend
an undersocore to the symbols and follows the MSVC x64
convention.  Therefore, any of the link libraries from
older toolchains are incompatible with the ones created
by these new builds.

////////////////////////////////////////////////////////////////////////////////////////////
OpenSSL for Windows
////////////////////////////////////////////////////////////////////////////////////////////

In earlier articles, we have looked at how to create a gcc build environment on Windows, and also how to compile binaries for Windows on Linux, using the MinGW-w64 suite to be able to support native 64-bit Windows builds.

But in order to build useful applications in these environments, we often need some common libraries. In this article, we will have a look at how to compile the OpenSSL library and make a small application that uses it. Compiled OpenSSL libraries are available for download (see the link at the bottom), in case you don’t want to do the compilation yourself.

 

prerequisites
We will be cross-compiling from Linux. If you want to use Windows only, please consider downloading the compiled OpenSSL binaries near the bottom of the page, or adjust the paths accordingly when building the library.

I have my 64-bit Windows build environment installed in /opt/mingw64, and the cross-compiler prefix is x86_64-w64-mingw32. I will target (build binaries for) 64-bit Windows in this article. Please adjust these variables according to your own build environment. i686-w64-mingw32 is the prefix for the 32-bit Windows cross-compiler.

 

compiling openssl
Follow the simple instructions on how to set up a Windows build environment on Linux. It is also possible to do this on Windows, but it is simpler and faster using Linux. Please leave a comment if you would like me to describe how to build on Windows.
Grab the desired OpenSSL source tarball. Use OpenSSL version 1.0.0 or newer; OpenSSL versions older than v1.0.0 are a bit harder to build on Windows, but let me know if you want to see how to do this.  I’ll use OpenSSL version 1.0.0e in the following, but the steps should be identical for any version newer than 1.0.0.
Put your tarball in a temporary directory, e.g. /tmp and unpack it:

	$ tar zxvf openssl-1.0.0e.tar.gz
	
Run the configure script to use the 64-bit Windows compiler.
	$ cd openssl-1.0.0e

	$ CROSS_COMPILE="x86_64-w64-mingw32-" ./Configure mingw64 no-asm shared --prefix=/opt/mingw64
…
Configured for mingw64.
Compile. Make sure the the cross-compiler is in your path, or add it explicitly as show below.

	$ PATH=$PATH:/opt/mingw64/bin make
…
Install it.

	$ sudo PATH=$PATH:/opt/mingw64/bin make install
	
We now have the OpenSSL libraries and headers for 64-bit Windows installed. Repeat the steps above with CROSS_COMPILE="i686-w64-mingw32-" and prefix /opt/mingw32 to build and install the 32-bit libraries for Windows.
 

A simple application

To confirm OpenSSL is working correctly, let’s create  a small C application that generates a SHA-256 digest of a character string. It reads a string given as the argument, generates the digest and shows the computed digest. The digest-generating code is shown below, while the complete code is available for download.

	void SHA256Hash(unsigned char digest[EVP_MAX_MD_SIZE], char *stringToHash)
	{
		OpenSSL_add_all_digests();

		const EVP_MD *md = EVP_get_digestbyname(“sha256”);

		EVP_MD_CTX context;
		EVP_MD_CTX_init(&context);
		EVP_DigestInit_ex(&context, md, NULL);
		EVP_DigestUpdate(&context, (unsigned char *)stringToHash, strlen(stringToHash));

		unsigned int digestSz;
		EVP_DigestFinal_ex(&context, digest, &digestSz);
		EVP_MD_CTX_cleanup(&context);

		EVP_cleanup();
	}

Save the file sha256.c in a working directory.
Compile it.

	$ /opt/mingw64/bin/x86_64-w64-mingw32-gcc -I/opt/mingw64/include -L/opt/mingw64/lib -Wall sha256.c -lcrypto -o sha256.exe
	
Check that the executable has the correct binary format (PE32+ is 64-bit).

	$ file sha256.exe
	
sha256.exe: PE32+ executable for MS Windows (console) Mono/.Net assembly
Copy our new program to a 64-bit Windows machine, and run it in the Windows Command Prompt.

	> sha256.exe 12345

The last step should generate the following dialog box, which contains the SHA-256 digest of the string “12345”.

	sha256.exe sample run

 

openssl windows binaries
In case you don’t want to compile the OpenSSL library yourself, I have compiled version 1.0.0e and made it available for download below.

OpenSSL 1.0.0e for 32-bit MinGW-w64 (prefix i686-w64-mingw32)
OpenSSL 1.0.0e for 64-bit MinGW-w64 (prefix x86_64-w64-mingw32)
Just unpack each tarball to your respective MinGW-w64 installation directory. They should work both if you are running the gcc compiler on Windows, as well as cross-compiling for Windows like we have done above.

Please leave a comment if you found this interesting or have suggestions for improvements!



