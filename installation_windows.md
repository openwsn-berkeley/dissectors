Build 6TiSCH Wireshark Dissector on Windows
===========================================

This file documents the installation of 6TiSCH Wireshark Dissector on a windows device. This page contains the steps to install 64-bit version and 32-bit version.

Workspace
---------
We assume the directory of workspace is C:\Development. 
First, we need clean up our workspace.

	rm -r C:\Development

Get Source code and extract it to workspace
---------------------------
- Download the wirehsark 2.0.3 source code and extract
 		
		wget https://1.eu.dl.wireshark.org/src/all-versions/wireshark-2.0.3.tar.bz2
		7z x wireshark-2.0.3.tar.bz2
		7z x wireshark-2.0.3.tar -oC:\Development
		mv C:\Development\wireshark-2.0.3 C:\Development\wireshark
		rm *.tar*


Install wireshark 64-bit
------------------------
- Open a windows command prompt
- Setup virtual studio environment

		call "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\amd64\vcvars64.bat" > nul

- Setup parameters
	
		set CYGWIN=nodosfilewarning
		set WIRESHARK_BASE_DIR=C:\Development
		set WIRESHARK_TARGET_PLATFORM=win64
		set QT5_BASE_DIR=C:\Qt\5.6\msvc2013_64
		set WIRESHARK_VERSION_EXTRA=-6tisch
		set WIRESHARK_CYGWIN_INSTALL_PATH=c:\cygwin64

- Add 6TiSCH dissector source

		cp packet* C:\Development\wireshark\epan\dissectors

- Compile the source code

		mkdir C:\Development\wsbuild64
		cd C:\Development\wsbuild64
		cmake -DENABLE_CHM_GUIDES=on -G "Visual Studio 12 Win64" ..\wireshark
		msbuild /m /p:Configuration=RelWithDebInfo Wireshark.sln

- Generate installer

		msbuild /m /p:Configuration=RelWithDebInfo nsis_package_prep.vcxproj
		msbuild /m /p:Configuration=RelWithDebInfo nsis_package.vcxproj

- Executable File and installer

	The executable file is located at: `C:\Development\wsbuild64\run\RelWithDebInfo\Wireshark.exe`

	The installer is located at: `C:\Development\wireshark\packaging\nsis\Wireshark-win64-2.0.3-6tisch.exe`

Install wireshark 32-bit
------------------------
- Open a windows command prompt
- Setup virtual studio environment

		call "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvars32.bat" > nul

- Setup parameters
	
		set WIRESHARK_BASE_DIR=C:\Users\user\Desktop\Development
		set WIRESHARK_TARGET_PLATFORM=win32
		set QT5_BASE_DIR=C:\Qt_x86\5.6\msvc2013
		set WIRESHARK_VERSION_EXTRA=-6tisch
		set WIRESHARK_CYGWIN_INSTALL_PATH=c:\cygwin64

- Add 6TiSCH dissector source

		cp packet* C:\Development\wireshark\epan\dissectors

- Compile the source code

		mkdir C:\Development\wsbuild32
		cd C:\Development\wsbuild32
		cmake -DENABLE_CHM_GUIDES=on -G "Visual Studio 12" ..\wireshark
		msbuild /m /p:Configuration=RelWithDebInfo Wireshark.sln

- Generate installer

		msbuild /m /p:Configuration=RelWithDebInfo nsis_package_prep.vcxproj
		msbuild /m /p:Configuration=RelWithDebInfo nsis_package.vcxproj

- Executable File and installer

	The executable file is located at: `C:\Development\wsbuild32\run\RelWithDebInfo\Wireshark.exe`

	The installer is located at: `C:\Development\wireshark\packaging\nsis\Wireshark-win32-2.0.3-6tisch.exe`