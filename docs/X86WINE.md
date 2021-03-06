# Installing Wine (and winetricks)
_TwisterOS users, take note: Wine, winetricks, and Box86 are pre-installed in TwisterOS. TwisterOS users do not have to install anything._

Using Wine with Box86 allows (x86) Windows programs to run on ARM Linux computers (x64 is not yet implemented).

Even though `wine-armhf` is available in many repo's on ARM devices (ie using _apt-get_ will attempt to install `wine-armhf` by default), `wine-armhf` will not work with Box86.  Box86 actually needs `wine-i386` to be installed manually on ARM devices instead.

_Note that manual installation is required, since using `multiarch` will result in your ARM device thinking it needs to install lots of i386 dependencies to make `wine-i386` work.  The "twist" in Box86 is that Box86 "wraps" many Windows and Wine i386 libraries (`.so` or `.dll` files) so that they will work with your ARM device's libraries.  Also note that wrapping libraries is an ongoing process throughout Box86 development and that some programs may not run properly until all of their i386 library dependencies are wrapped._

Wine requires a 3G/1G split memory kernel.  The Pi 4 has this 3G/1G split, but **the Pi 3 and earlier models have a 2G/2G kernel by default and will need a custom-compiled 3G/1G kernel to get Wine to work.**  Botspot's [pi-apps](https://github.com/Botspot/pi-apps/) installer can install Box86, Wine, winetricks, and a new kernel for you if you are running Raspberry Pi 3.  You may also be able to google some instructions on how to make a custom 3G/1G kernel for your Pi.

## Overview and Notes
The general procedure for installing Wine for Box86 is to...
 - Download all the install files (.deb, .zip, or even .pol files) for the version of Wine you wish to install
 - Unzip or dpkg the install files into one folder
 - Move that folder to the directory that you wish Wine to run from (in TwisterOS, this is `~/wine/` by default)
 - Go to `/usr/local/bin` and make symlinks or scripts that will point to your main wine binaries (`wine`, `winecfg`, and `wineserver`).
 - Boot wine to create a new wineprefix
 - Download winetricks (which is simply a very complicated bash script), make it executable, then copy it to `/usr/local/bin`

Your entire Wine installation can reside within a single folder on your Linux computer.  TwisterOS assumes that your Wine installation is located inside the `~/wine/` directory.  The actual directory where you put your `wine` folder doesn't matter as long as you have symlinks within the `/usr/local/bin/` directory which point to the `wine` folder so that Linux can find Wine when you type `wine` into the terminal).  When you first run or boot Wine (`wine wineboot`), Wine will create a new user environment within which to install Windows software.  This user environment is called a "wineprefix" (or "wine bottle") and is located (by default) in `~/.wine` (note that Linux folders with a `.` in front of them are "hidden" folders).  For more Wine documentation, see [WineHQ](https://www.winehq.org/documentation).

Some versions of Wine work better with certain software.  It is best to install a version of Wine that is known to work with the software you would like to run.  There are three main development branches of Wine you can pick from, referred to as wine-stable, wine-devel, and wine-staging.  The wine-staging branch requires extra installation steps on Raspberry Pi.

Install files for Wine can be found from the [TwisterOS FAQ](https://twisteros.com/faq.html) page, the PlayOnLinux website, or the WineHQ repository.  Box86 requires the "i386" versions of Wine install files (even though we are installing it on an ARM processor).  Box86 "wraps" many of Wine's core Linux i386 libraries so that their calls are interpretable by Linux ARM libraries.  Below are examples of how to install Wine from each of those places.

Winetricks is a script which makes it easier to install & configure any desired Windows core system software packages which may be dependencies for certain Windows programs.


## Examples

### Installing wine-devel, wine-stable, or wine-staging for Raspberry Pi OS Buster from WineHQ deb files
_Links from https://dl.winehq.org/wine-builds/debian/dists/buster/main/binary-i386/_
```
# Backup any old wine installations
sudo mv ~/wine ~/wine-old
sudo mv ~/.wine ~/.wine-old
sudo mv /usr/local/bin/wine /usr/local/bin/wine-old
sudo mv /usr/local/bin/winecfg /usr/local/bin/winecfg-old
sudo mv /usr/local/bin/wineserver /usr/local/bin/wineserver-old

# Download and extract wine
# (Replace the links/versions below with links/versions from the WineHQ site for the version of wine you wish to install. Note that we need the i386 version for Box86 even though we're installing it on our ARM processor.)
# (Pick an i386 version of wine-devel, wine-staging, or wine-stable)
cd ~/Downloads
wget https://dl.winehq.org/wine-builds/debian/dists/buster/main/binary-i386/wine-devel-i386_5.21~buster_i386.deb # NOTE: Replace this link with the version you want
wget https://dl.winehq.org/wine-builds/debian/dists/buster/main/binary-i386/wine-devel_5.21~buster_i386.deb  # NOTE: Also replace this link with the version you want
dpkg-deb -xv wine-devel-i386_5.21~buster_i386.deb wine-installer # NOTE: Make sure these dpkg command matches the filename of the deb package you just downloaded
dpkg-deb -xv wine-devel_5.21~buster_i386.deb wine-installer
rm wine*.deb # clean up

# Install wine (move wine folder into place, make launcher, & make symlinks. Credits: grayduck, Botspot)
mv ~/Downloads/wine-installer/opt/wine* ~/wine
echo -e '#!/bin/bash\nsetarch linux32 -L '"$HOME/wine/bin/wine "'"$@"' | sudo tee -a /usr/local/bin/wine >/dev/null # Create a script to launch wine programs as 32bit only
sudo ln -s ~/wine/bin/wineserver /usr/local/bin/wineserver
sudo ln -s ~/wine/bin/wineboot /usr/local/bin/wineboot
sudo ln -s ~/wine/bin/winecfg /usr/local/bin/winecfg
sudo chmod +x /usr/local/bin/wine /usr/local/bin/wineboot /usr/local/bin/wineserver /usr/local/bin/winecfg
rm -rf wine-installer # clean up

# These packages are needed for running wine-staging on RPi 4 (Credits: chills340)
sudo apt install libstb0 -y
cd ~/Downloads
wget http://ftp.us.debian.org/debian/pool/main/f/faudio/libfaudio0_20.11-1~bpo10+1_i386.deb
wget -r -l1 -np -nd -A "libfaudio0_*~bpo10+1_i386.deb" http://ftp.us.debian.org/debian/pool/main/f/faudio/ # Download libfaudio i386 no matter its version number
dpkg-deb -xv libfaudio0_*~bpo10+1_i386.deb libfaudio
sudo cp -TRv libfaudio/usr/ /usr/
rm libfaudio0_*~bpo10+1_i386.deb # clean up
rm -rf libfaudio # clean up

# Boot wine (make fresh wineprefix in ~/.wine )
wine wineboot
```

### Installing Wine for Raspberry Pi OS from the Twister OS FAQ page
_Links from https://twisteros.com/faq.html_
```
# Backup any old wine installations
sudo mv ~/wine ~/wine-old
sudo mv ~/.wine ~/.wine-old
sudo mv /usr/local/bin/wine /usr/local/bin/wine-old
sudo mv /usr/local/bin/winecfg /usr/local/bin/winecfg-old
sudo mv /usr/local/bin/wineserver /usr/local/bin/wineserver-old

# Download and extract wine (last I checked, the Twister OS FAQ page had Wine 5.13-devel)
wget https://twisteros.com/wine.tgz -O ~/wine.tgz
tar -xzvf ~/wine.tgz
rm ~/wine.tgz # clean up

# Install wine (move wine folder into place, make launcher, & make symlinks. Credits: grayduck, Botspot)
echo -e '#!/bin/bash\nsetarch linux32 -L '"$HOME/wine/bin/wine "'"$@"' | sudo tee -a /usr/local/bin/wine >/dev/null # Create a script to launch wine programs as 32bit only
sudo ln -s ~/wine/bin/wineserver /usr/local/bin/wineserver
sudo ln -s ~/wine/bin/wineboot /usr/local/bin/wineboot
sudo ln -s ~/wine/bin/winecfg /usr/local/bin/winecfg
sudo chmod +x /usr/local/bin/wine /usr/local/bin/wineboot /usr/local/bin/wineserver /usr/local/bin/winecfg

# Boot wine (make fresh wineprefix in ~/.wine )
wine wineboot
```

## Wineprefixes (and Wine initialization)
The first time Wine is run (`wine wineboot`), it will create a fresh wineprefix for you (by default, located in the hidden folder `~/.wine`).  Think of a wineprefix as Wine's virtual 'harddrive' where it installs software and saves settings.  Wineprefixes are portable and deletable.

If you at any point corrupt something inside your default wineprefix, you can start "fresh" by deleting your `~/.wine` directory (with the `rm -rf ~/.wine` command) and boot wine again to create a new default wineprefix.

## Transplanting wineprefixes (side-loading)
If software isn't installing in Wine with Box86, but is installing for you in Wine on a regular x86 Linux computer, you can copy a wineprefix from your x86 Linux computer to the device you're running Box86 on.  This is most easily done by tarring the `~/.wine` folder on your x86 Linux computer (`tar -cvf winebottle.tar ~/.wine`), transferring the tar file to your device, then un-tarring the tar file on your device running Box86 & Wine.  Tarring the wineprefix preserves any symlinks that are inside it.

## Swapping out different versions of Wine
You can change which version of Wine you are running simply by renaming your old `wine` and `.wine` folders to something else, then putting a new `wine` folder (containing your new version of Wine) in its place. Running `wine wineboot` again will make a fresh wineprefix with your new version of Wine.  You can check which version of Wine you're running with the `wine --version` command.

## Installing and Using winetricks
```
# Backup old winetricks
sudo mv /usr/local/bin/winetricks /usr/local/bin/winetricks-old

# Download
cd ~/Downloads
wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks

# Install
sudo chmod +x winetricks
sudo cp winetricks /usr/local/bin

# winetricks needs this installed
sudo apt-get install cabextract -y
```
Whenever we run winetricks, we must suppress Box86's banner by typing `BOX86_NOBANNER=1` to avoid errors.  Similarly, invoking Box86's logging features (with `BOX86_LOG=1` or similar) will cause winetricks to crash (unless we patch winetricks - see the *Troubleshooting* section) # future work.

Here is an example of a winetricks command:
`BOX86_NOBANNER=1 winetricks -q corefonts vcrun2010 dotnet35sp1`