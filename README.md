from http://www.carrier-lost.org/raspberry-pi-truecrypt-on-raspbian/

Raspberry Pi: Truecrypt on Raspbian
16 Antworten
Quick howto on how to install truecrypt on the rapberry pi.

Get the source for Mac OS X/Linux from http://www.truecrypt.org/downloads2 (Update: https://www.grc.com/misc/truecrypt/truecrypt.htm) and copy the file to the pi:

pat@earth Downloads]$ scp TrueCrypt 7.1a Source.tar.gz pi@pi:/home/pi/

Connect to the pi and extract the archive:

pi@raspberrypi ~ $ tar xfv TrueCrypt 7.1a Source.tar.gz

Even without a GUI, you’ll need WxWidget. Download and extract:

pi@raspberrypi ~ $ wget http://prdownloads.sourceforge.net/wxwindows/wxWidgets-2.8.12.tar.gz
pi@raspberrypi ~ $ tar xfv wxWidgets-2.8.12.tar.gz

Install the fuse library:

pi@raspberrypi ~ $ sudo aptitude install libfuse-dev

Create a folder and download some needed header files:

pi@raspberrypi ~ $ mkdir ~/truecrypt-7.1a-source/pkcs
pi@raspberrypi ~ $ wget ftp://ftp.rsasecurity.com/pub/pkcs/pkcs-11/v2-20/*.h -P truecrypt-7.1a-source/pkcs/

Change to the truecrypt directory and compile WxWidgets (takes about 20 minutes):

pi@raspberrypi ~ $ cd truecrypt-7.1a-source/
pi@raspberrypi ~/truecrypt-7.1a-source $ export PKCS11_INC=/home/pi/truecrypt-7.1a-source/pkcs/
pi@raspberrypi ~/truecrypt-7.1a-source $ make NOGUI=1 WX_ROOT=/home/pi/wxWidgets-2.8.12 wxbuild

Now compile truecrypt (~ 40 minutes):

pi@raspberrypi ~/truecrypt-7.1a-source $ make NOGUI=1 WXSTATIC=1

Copy the binary into the bin directory:

pi@raspberrypi ~/truecrypt-7.1a-source $ sudo cp Main/truecrypt /usr/local/bin/

Mount your container:

pi@raspberrypi ~/truecrypt-7.1a-source $ truecrypt -t -k „“ –protect-hidden=no –mount /mnt/usb/crypt /mnt/truecrypt/ -m=nokernelcrypto

Cleanup:

pi@raspberrypi ~/truecrypt-7.1a-source $ cd ~
pi@raspberrypi ~ $ rm -r truecrypt-7.1a-source TrueCrypt 7.1a Source.tar.gz wxWidgets-2.8.12*
