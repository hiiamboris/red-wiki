### About

This page lists dependencies for major Linux distributions required by Red/View GTK backend.

Core dependencies are listed on the official [download page](https://www.red-lang.org/p/download.html).

If you don't need a View module, compile the console with `--no-view` flag (see `--help` output for more info).

### Debian

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