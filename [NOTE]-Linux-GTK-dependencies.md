### About

This page lists dependencies for major Linux distributions required by Red/View GTK backend.

Core dependencies are listed on the official [download page](https://www.red-lang.org/p/download.html).

### Debian/Ubuntu

#### 64-bit

```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install -y libgtk-3.0:i386 libcanberra-gtk3-module:i386
```

#### 32-bit

```
sudo apt-get update
sudo apt-get install -y libgtk-3.0 libcanberra-gtk3-module
```

### CentOS

#### 64-bit

```
yum install -y gtk3.i686
```

#### 32-bit

```
yum install -y gtk3
```

### Void Linux

#### 64-bit
```
xbps-install -y void-repo-multilib
xbps-install -S
xbps-install -y glibc-32bit libcurl-32bit gdk-pixbuf-32bit
```

### Armbian

Tested on Armbian_22.08.0_Aml_s905l3a_bullseye_5.15.62_server

#### AArch64
1. Add armhf support
```
sudo dpkg --add-architecture armhf
sudo apt-get update
sudo apt-get install libc6:armhf
```
2. Install libs for Red CLI
```
sudo apt-get install libcurl4:armhf
sudo apt-get install libgdk-pixbuf-2.0-0:armhf
```
3. If you want to run view app, install the following libs
```
sudo apt-get install libgtk-3-0:armhf
```