# 6TiSCH/IEEE802.15.4e  and 6LoWPAN Wireshark dissectors

Important: latest Wireshark version (from 2.5.0-370-ga3b4731eb) has everything you need to see all the details contained in the 6TiSCH frames.

Note: This dissector is developed based on Wireshark 2.0.3.

These dissectors extend the Wireshark IEEE802.15.4 dissectors with the following:

* dissect the IEEE802.15.4 FCF for "version 2" frames (new in IEEE802.15.4e-2012)
* dissect all Information Elements used in draft-6tisch-minimal
* dissect IP-in-IP encapsulation
* dissect the 6lo Paging and Routing Dispatch in 6LoWPAN
* dissect the 6P protocol frame defined in draft-ietf-6tisch-6top-protocol

The source code of those dissectors is available as open-source code (BSD license). Although we are making final adjustments, you can use the code today. For now, you have to recompile Wireshark (instructions below); we will work on submitting this code to the Wireshark project once we are finished.

## Build status
|              builder                                                           |      build                 | outcome
| ------------------------------------------------------------------------------ | -------------------------- | -------
| [OpenWSN builder](http://builder.openwsn.org/job/6TiSCH%20Wireshark/)          | compile (6TiSCH Wireshark) | [![Build Status](http://openwsn-builder.paris.inria.fr/buildStatus/icon?job=6TiSCH Wireshark)](http://builder.openwsn.org/job/6TiSCH%20Wireshark/)

## On Windows

The following are the steps to install the dissector on a Windows machine.
It has been tested on a computer with Windows 8.1-64-bit version installed.

### Workspace

We assume the directory of workspace is C:\Development. The wireshark source code will be extracted at this directory.

### Toolchain installation

Follow https://www.wireshark.org/docs/wsdg_html_chunked/ChSetupWin32.html to install the toolchain.

### Get Source code and extract it to workspace
- Download the wirehsark 2.0.3 source code and extract
        
        wget https://1.eu.dl.wireshark.org/src/all-versions/wireshark-2.0.3.tar.bz2
        7z x wireshark-2.0.3.tar.bz2
        7z x wireshark-2.0.3.tar -oC:\Development
        mv C:\Development\wireshark-2.0.3 C:\Development\wireshark
        rm *.tar*


- Get 6TiSCH dissector source code and extract to wireshark source code

        cd C:\Development\
        git clone https://github.com/openwsn-berkeley/dissectors.git
        cd dissectors
        cp packet* C:\Development\wireshark\epan\dissectors

### Install wireshark 64-bit
- Open a windows command prompt
- Setup virtual studio environment for 64-bit version by calling vcvars64.bat

        call "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\amd64\vcvars64.bat" > nul

*Note: the command here uses the default installing directory when installs Virtual studio 2013. Change it accordingly if you have VS 2013 installed somewhere else.*

- Setup parameters
    
        set CYGWIN=nodosfilewarning
        set WIRESHARK_BASE_DIR=C:\Development
        set WIRESHARK_TARGET_PLATFORM=win64
        set QT5_BASE_DIR=C:\Qt\5.6\msvc2013_64
        set WIRESHARK_VERSION_EXTRA=-6tisch
        set WIRESHARK_CYGWIN_INSTALL_PATH=c:\cygwin64

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

### Install wireshark 32-bit
- Open a windows command prompt
- Setup virtual studio environment for 32-bit version by calling vcvars32.bat

        call "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvars32.bat" > nul

- Setup parameters
    
        set WIRESHARK_BASE_DIR=C:\Users\user\Desktop\Development
        set WIRESHARK_TARGET_PLATFORM=win32
        set QT5_BASE_DIR=C:\Qt_x86\5.6\msvc2013
        set WIRESHARK_VERSION_EXTRA=-6tisch
        set WIRESHARK_CYGWIN_INSTALL_PATH=c:\cygwin64

- Get 6TiSCH dissector source code and extract to wireshark source code
    
        cd C:\Development\
        git clone https://github.com/openwsn-berkeley/dissectors.git
        cd dissectors
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

- Artifacts built by OpenWSN builder

    [Wireshark-win32-2.0.3-6tisch.exe](http://builder.openwsn.org/job/6TiSCH%20Wireshark/lastSuccessfulBuild/artifact/Wireshark-win32-2.0.3-6tisch.exe)

    [Wireshark-win64-2.0.3-6tisch.exe](http://builder.openwsn.org/job/6TiSCH%20Wireshark/lastSuccessfulBuild/artifact/Wireshark-win64-2.0.3-6tisch.exe "Wireshark-win64-2.0.3-6tisch.exe")

## on Linux (Ubuntu)

### get the source code of dissector and wiresark

    cd Desktop
    wget https://1.eu.dl.wireshark.org/src/all-versions/wireshark-2.0.3.tar.bz2
    tar xaf wireshark-2.0.3.tar.bz2
    cd wireshark-2.0.3/epan
    mv dissectors dissectors_old
    git clone https://github.com/openwsn-berkeley/dissectors.git
    cp -a --no-clobber dissectors_old/. dissectors
    cd ..

### Dependencies installation

    sudo apt-get install -y qt5-default
    sudo apt-get install -y libqt5multimedia5 libqt5multimediawidgets5
    sudo apt-get install -y libgtk-3-dev
    sudo apt-get install -y libpcap0.8-dev
    sudo apt-get install -y bison flex

### Configure and make

    ./configure
    make

### Run dissector and create deb package

To run the built wireshark 

    cd ~/Desktop/wireshark
    ./wireshark

To create the deb package

    cd ~/Desktop/wireshark
    dpkg-buildpackage -us -uc -rfakeroot
