## FREEBSD 14
here is my freebsd 14 envirment setup,with this setup i am sure all packages work  
1.install some basic depends
```
sudo pkg ins android-tools liblz4 unrar libnotify 7-zip exfat-utils qt5 qt6 neofetch git cmake wget fakeroot dpkg mkvalidator mkclean libheif protobuf grpc mbedtls libimobiledevice usbmuxd fusefs-ifuse fusefs-ntfs fusefs-ext2 fusefs-exfat fusefs-smbnetfs vim wx32-gtk3 yarn glx-utils pciutils usbutils libva-utils pkgconf gmake meson gcc13 boost-all libplist help2man autoconf m4 jq gettext openssl sqlite3 rpm4 ja-eb libzim xapian-core R-cran-hunspell lzo2 zh-opencc libvorbis py39-python-xlib py39-ipython
```
2.switch frome python3.9 to python3.11,because python3.11 is not complete in offical repo
```
su

pkg install python311 py311-sqlite3
ln -s /usr/local/bin/python3.11 /usr/local/bin/python3
ln -s /usr/local/bin/python3.11 /usr/local/bin/python
python -m ensurepip --upgrade
pip3 install --upgrade pip
pip3 install PyGObject PyQt-builder PyQt5-sip 

## pyqt5 with root!
download source file https://pypi.org/project/PyQt5/#files
sip-install

exit
```
3.install some pyhon311 modules
```
sudo pip install PySocks colorama distro polib xxhash Sphinx semantic-version Send2Trash psutil Pillow mutagen pyinstaller icoextract altgraph crccheck pltable google-translate-for-goldendict
```
4.enbale linux-c7,my package freefilesync and motrix need it
```
sudo sysrc linux_enable="YES"
sudo service linux start
sudo pkg install linux-c7 linux-c7-gtk3 

sudo bash -c 'echo "
#linux
devfs      /compat/linux/dev      devfs      rw,late                    0  0
tmpfs      /compat/linux/dev/shm  tmpfs      rw,late,size=1g,mode=1777  0  0
fdescfs    /compat/linux/dev/fd   fdescfs    rw,late,linrdlnk           0  0
linprocfs  /compat/linux/proc     linprocfs  rw,late                    0  0
linsysfs   /compat/linux/sys      linsysfs   rw,late                    0  0
" >> /etc/fstab'
```
reboot

# PREPARE
1.clone this repo to local and install dpkg for freebsd
``` 
  sudo pkg ins dpkg
  sudo mkdir -p /var/db/dpkg
  sudo touch /var/db/dpkg/status
```

2.install a tool
```
su

echo '#!/bin/sh
install_packages() {
    for inputfile in "$@"; do
        if [ -f "$inputfile" ]; then
            sudo dpkg -i --force-all "$inputfile"
        else
            echo "Error: File '$inputfile' does not exist."
        fi
    done
}

update_icon_cache() {
    sudo gtk-update-icon-cache -f /usr/local/share/icons/hicolor/
}

if [ "$#" -eq 0 ]; then
    echo "Usage: $0 <inputfile1> [inputfile2] [inputfile3] ..."
    exit 1
fi

install_packages "$@"
update_icon_cache
echo "Package installation and icon cache update completed."' > /usr/local/bin/dpkgforce && chmod +x /usr/local/bin/dpkgforce

exit
```
# INSTALL
dpkgforce *.deb
