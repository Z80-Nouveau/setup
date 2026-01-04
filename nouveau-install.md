# Installing everything needed for Z80-Nouveau development

Other than for the task of programming the FPGA with a Raspberry PI, the following should work on most Linux systems.
You might have to change the `apt` commands to use your system's package manager if you are not using a Debian style system.

The following commands have been verified on a Raspberry PI configured as described here: https://github.com/johnwinans/raspberry-pi-install

```
sudo apt install build-essential libi2c-dev z80asm cpmtools
sudo apt install iverilog fpga-icestorm yosys nextpnr-ice40 flashrom
#sudo apt install gtkwave kicad          # not normally needed on a headless system
sudo apt install minicom

# Use these for anonymous downloading (if you don't have a github account), else change to your own forks
GIT_RETRO=http://github.com/Z80-Retro
GIT_WINANS=http://github.com/johnwinans

#GIT_RETRO=git@github.com:Z80-Retro
#GIT_WINANS=git@github.com:johnwinans

cd ~
mkdir ~/fpga
cd ~/fpga
git clone --recurse-submodules $GIT_RETRO/2063-Z80-cpm.git
git clone $GIT_RETRO/example-filesystem.git
git clone $GIT_WINANS/2057-ICE40HX4K-TQ144-breakout.git
git clone $GIT_WINANS/2067-Z8S180.git
git clone $GIT_WINANS/Verilog-Examples.git

# make sure everything works:
cd ~/fpga/2063-Z80-cpm

cat - > Make.local <<EOF
CROSS_AS_FLAGS=-I../lib -I../libnouveau
BOOT_MSG=Z80 Nouveau
SD_HOSTNAME=nouveau
SD_DEV=/dev/sdb1
EOF

make world

cd ~/fpga/2067-Z8S180/fpga/nouveau-vdp99
make world
make prog
minicom -D /dev/ttyAMA0
```
