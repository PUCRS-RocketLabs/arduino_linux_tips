## Linux/Arduino tips

The Arduino team does a big effort putting together lots of tools, assuring
everything works properly together and distribiting it on multiple hardware
platforms in an easy way for the average user to install and use it.
They even provide an easy way to download libraries and programmers for
differnt microcontroler boards.

Therefore, there are some cavets... The compile time when using Arduino IDE
is generaly slow, and the keybindings within the IDE are different from your
favorite programming editor, eeder it being vi, Atom, Visual Studio Code,
GnuEmacs or any other editor programmers use to choose.

For those who love Arduino and while coding for the platform at a daylly
basis are also unwilling to leave their favorite coding editors...
[Suadar Muthu](https://github.com/sudar) brought to us the 
[Arduino Makefile](https://github.com/sudar/Arduino-Makefile)

[Arduino Makefile](https://github.com/sudar/Arduino-Makefile) and some
Unix/Linux features like symolic links would help to build
a neat and simple arduino development environment.

Although most linux distributions are bundled with a package for the
Arduino and for the arduino-mk, keeping your set of libraries can
be a tuf task... I this scenario, some Linux tips may come in hand.

## Install your distribution Arduino and arduino-mk native packages

The first step is to install the distribution native packages for the
Arduino sofware and IDE and the arduino-mk package. Particularly for
the Ubuntu this can be done with this line:

```
sudo apt install arduino-mk arduino
```

And run the arduino IDE for the first time so it can create the
arduino preferences file inside your home folder
(``~/.arduino/preferences.txt``)

This would install any dependency those softwares need and will also
properlly setup the USB and serial ports.

## Locally download the latest Arduino and arduino-mk version

Those packages are relatively small and one simple way to keep up with
the updates is to have a local folder inside your home folder to keep
the latest versions. For example for the Arduino version 1.8.12


```
mkdir ~/local
cd local
wget https://downloads.arduino.cc/arduino-1.8.12-linux64.tar.xz
tar xvf arduino-1.8.12-linux64.tar.xz
ln -s arduino-1.8.12 latest
git clone https://github.com/sudar/Arduino-Makefile.git
```

This creates a local folder at your home folder.
Inside this folder you dowload the latest arduino software, in this
example is the version 1.8.12, but usually to change the version you
just need to replace 1.8.12 with the latest version.

After thar, the software is unpacked and a unix symbolic link is created,
so you can always refere to the arduino installation path using this link,
pointing to the latest version.

The Arduino Makefile repository just need to clonned. No instalation is
necessary.

## Create your arduino sketches inside the regualar sketches folder

Inside the folder of a particular stech, you may place a ```Makefile```:

```
## Comments on BOARD_SUB on Arduino 1.6.X
## https://github.com/sudar/Arduino-Makefile/issues/398
BOARD_TAG = mega
BOARD_SUB = atmega2560
#BOARD_TAG = nano
#BOARD_SUB = atmega328
ARDUINO_DIR = $(HOME)/local/latest
#ARDUINO_SKETCHBOOK =  $(HOME)/Dropbox/sketchbook
ARDUINO_SKETCHBOOK =  $(HOME)/sketchbook
ARDUINO_LIBS = SD SPI RTClib Wire #LiquidCrystal 
#MONITOR_PORT = /dev/ttyUSB0
MONITOR_PORT = /dev/ttyACM0
CFLAGS_STD = -Wno-write-strings -std=gnu99
include $(HOME)/local/Arduino-Makefile/Arduino.mk
#include /usr/share/arduino/Arduino.mk

check-syntax:
        $(CXX) -c -include Arduino.h   -x c++ $(CXXFLAGS)   $(CPPFLAGS)  -fsyntax-only $(CHK_SOURCES)

clean::
        @rm -rfv *~
```

## Explaning this template Makefile

### ARDUINO_DIR

This particular Makefile points to the latest Arduino sofware previouslly
dowloaded and installed in the ($HOME)/local folder.

### clean


