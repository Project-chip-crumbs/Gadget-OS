# gadgetos

## Install required build tools

#### Install sunxi-fel and fastboot: Clients for bootloaders on gr8. These support USB flashing.
#### linux (debian):
```
# TODO: fastboot install instructions
<install android-platform-tools>

# get sunxi-fel
sudo apt-get update
sudo apt-get install libusb-1.0-0-dev pkg-config
git clone https://github.com/linux-sunxi/sunxi-tools.git
pushd sunxi-tools
make
make install
popd
```
#### macOS:
```
# get fastboot
brew install android-platform-tools

# get sunxi-fel
git clone https://github.com/linux-sunxi/sunxi-tools.git
pushd sunxi-tools
make
make install
popd
```

#### Install docker: Provides containers to run your gadgetos builds
#### linux (debian):
https://docs.docker.com/engine/installation/linux/debian/
#### macOS:
https://docs.docker.com/docker-for-mac/

## Build and flash gadgetos

#### Get gadgetos source code
```
git clone https://github.com/nextthingco/gadget-os-proto
cd gadget-os-proto
git submodule update --init --recursive
```

#### Create docker image for building gadgetos
```
scripts/build-container
```

On OS X, you'll need to launch the Docker application from your Applications folder before you can run these scripts. This can take about 10-15 minutes, depending on your computer.

#### Build gadgetos base buildroot defconfig
```
scripts/build-gadget make chippro_defconfig
```

This takes less than a minute.

#### [optional] Override base defconfig using ncurses utility. You'll know if you need this.
```
scripts/build-gadget make nconfig
```
#### Build gadgetos
```
scripts/build-gadget make
```

This can take an hour or more

#### Start UART terminal

Open a new terminal window, plug in to the dev kit's USB micro connector and start a terminal session:
```
screen /dev/tty.usbserial-XXX 115200 #macOS - hit tab after the '-'
screen /dev/ttyUSB0 #linux
```

#### Flash gadgetos image to chippro
Hold down the fel button and power up the Dev Board and
```
scripts/flash-gadget
```
In the other UART terminal window you'll need to enter a couple commands to get things going. When you get to the `=>` prompt enter:
```
=> fastboot 0
```
You'll get that prompt again later, then you'll need to:
```
=> reset
```

#### Alternatively
If the dev kit already has a gadget OS on it and you are re-flashing, boot CHIP Pro, then open a terminal. Use the command:
```
gadget-enter-flashing-mode
```
This will reboot into a flashing mode. From your computer's command line in the gadget-os-proto docker, run the flash script:
```
scripts/flash-gadget
```
It will complaing that FEL device is not found, then after a few seconds, begin flashing from fastboot. 
