# 6TiSCH/IEEE802.15.4e Wireshark dissectors

Note: developed for Wireshark 1.99.7

This dissector is for:

* IEEE802.15.4e
    * the updated FCF
    * the following Information Elements
        * TODO

# cloning

Clone this repo in your Wireshark source code so you can push/pull changes and easy recompile without moving files around.

* download the "Development Release" source code from https://www.wireshark.org/download.html
* unzip
* rename `wireshark-1.XX.X\epan\dissectors\` top `wireshark-1.XX.X\epan\dissectors_old\`
* clone the repo in `wireshark-1.XX.X\epan\` and rename the clone to `dissectors\`
* move all the contents of `wireshark-1.XX.X\epan\dissectors_old\` into `wireshark-1.XX.X\epan\dissectors\`
* The `.gitignore` is set up in such a way that you will not commit any code other than the IEEE802.15.4 dissector

# compiling

## on Windows

Follow https://www.wireshark.org/docs/wsdg_html_chunked/ChSetupWin32.html to install the toolchain.

On Windows 8.1 open the VS2013 shell by:
* looking for "visual"
* select "Visual Studio Tools"
* selecting "VS2013 x64 Native Tools Command Prompt"

Prepare the environment, verify installation, setup and build:

```
set CYGWIN=C:\cygwin64
set WIRESHARK_BASE_DIR=C:\wireshark\wireshark-1.99.7
set WIRESHARK_TARGET_PLATFORM=win64
set QT5_BASE_DIR=C:\Qt\Qt5.4.2\5.4\msvc2013_64_opengl
set WIRESHARK_VERSION_EXTRA=-tw
set PATH=C:\cygwin64\bin;%PATH%
cd C:\wireshark\wireshark-1.99.7

nmake -f Makefile.nmake verify_tools
nmake -f Makefile.nmake setup
nmake -f Makefile.nmake all

```

## on Linux (Ubuntu)

* go to `wireshark/` folder
* make sure you have all dependencies.
    * Install `Qt5`
        * `sudo apt-get install qt5-default`
        * `sudo apt-get install libqt5multimedia5 libqt5multimediawidgets5`
    * Install `GTK3`
        * `sudo apt-get install libgtk-3-dev`
    * Install `libpcap`
        * `sudo apt-get install libpcap0.8-dev `
* Confiure and build
    * `./configure`
    * `./make`
* Run 
    * `./wireshark`
* Or create the deb package
    * `dpkg-buildpackage -us -uc -rfakeroot`
