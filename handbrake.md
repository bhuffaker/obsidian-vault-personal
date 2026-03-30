#dvd #handbrake

You need to download from: 
https://handbrake.fr/rotation.php?file=HandBrake-1.8.2.dmg

Then install libdvdcss.2.dylib
~~~
brew install libdvdcss
~~~

## Error logs
/Users/bhuffake/Library/Containers/fr.handbrake.HandBrake/Data/Library/Application\ Support/HandBrake/HandBrake-activitylog.txt

## libdvdcss
~~~
git clone https://code.videolan.org/videolan/libdvdcss.git
autoreconf -i
 ./configure --prefix=/usr/local
sudo make install
~~~

## installing  (didn't solve the problem)
- Rename /Applications/Handbrake.app as [Handbrake-x64.app](https://handbrake-x64.app/).
- Right-click Handbrake-x64.app, select "Get Info", then check the box for "Open using Rosetta"
- brew uninstall libdvdcss
- Right-click /Applications/Utilities/Terminal.app, select "Duplicate", name it [Terminal-x64.app](https://terminal-x64.app/)
- Right-click Terminal-x64.app, select "Get Info", then check the box for "Open using Rosetta"
- Open [Terminal-x64.app](https://terminal-x64.app/)
- run 'arch' and make sure it says 'i386', not 'arm64'
- cd /tmp
- Downloqd libdvdcss-1.4.3.tar
	- curl -OL [https://get.videolan.org/libdvdcss/1.4.3/libdvdcss-1.4.3.tar.bz2](https://get.videolan.org/libdvdcss/1.4.3/libdvdcss-1.4.3.tar.bz2)
	- cat libdvdcss-1.4.3.tar.bz2 | bunzip2 | tar x
- cd libdvdcss-1.4.3
- autoreconf -vif
- ./configure --prefix=/usr/local
- make
- sudo make install
- Now open [Handbrake-x64.app](https://handbrake-x64.app/) and your DVDs should decode correctly.