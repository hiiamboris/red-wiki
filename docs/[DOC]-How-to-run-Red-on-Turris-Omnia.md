[Turris Omnia](https://omnia.turris.cz/en/) is very cool open source router that is running modified version of [OpenWRT](https://openwrt.org/). It is possible to run Red there, compile it with -RPi as target. You also need to make small change on Turris, because it uses [musl](https://www.musl-libc.org/). You need to make a symlink:

`ln -s /lib/ld-musl-armhf.so.1  /lib/ld-linux-armhf.so.3`

Now Red should run just fine, until you try to read file ;)