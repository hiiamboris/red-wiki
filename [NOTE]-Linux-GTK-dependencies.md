### About

This page lists dependencies for major Linux distributions required by Red/View GTK backend.

### Debian

#### 64-bit
```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install -y libgtk-3-bin:i386 librsvg2-common:i386 libcanberra-gtk-module:i386 libcanberra-gtk3-module:i386 at-spi2-core:i386
sudo apt-get install -y dbus-x11:i386
```

#### 32-bit

```
sudo apt-get update
sudo apt-get install -y libgtk-3-bin librsvg2-common libcanberra-gtk-module  libcanberra-gtk3-module  at-spi2-core 
sudo apt-get install -y dbus-x11
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