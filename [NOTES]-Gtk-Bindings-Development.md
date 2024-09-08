# GTK Bindings Development

Development of Linux backend for [Red's Graphical User Interface](https://doc.red-lang.org/en/gui.html), using version 3 of [Gtk+ library](https://en.wikipedia.org/wiki/GTK%2B).

* Sources: https://github.com/red/red/tree/GTK (official Red repo, GTK branch)
* Gitter channel (chat, help): [red/GTK](https://rebol.tech/gitter.im/red/GTK)
* Precompiled binaries:
  * https://static.red-lang.org/dl/branch/GTK/linux/red-latest – official dev builds for x86 architecture
  * https://rebolek.com/builds/ – community nightly builds by @rebolek, x86 and ARM (Raspberry Pi)
  * `red-gtk` binary (equivalent of `red` binary but for rcqls/GTK-dev branch, may be outdated) provided [here](https://cqls.dyndoc.fr/users/RCqls/Red/red-gtk).

[Official repo's GTK branch](https://github.com/red/red/tree/GTK) is now a default development branch for GTK implementation. It moved from @rcqls' development branches, which [are no longer a reference point](https://rebol.tech/gitter.im/red/GTK/2019/#msg5d4e965690bba62a124e933b).

## Compilation (32-bit Linux)

(see [rcqls/docker-red-gtk Dockerfile](https://github.com/rcqls/docker-red-gtk/blob/master/Dockerfile) or [`reds project` page](https://github.com/rcqls/reds/blob/master/README-RedGTK.md) for quick installation notes about `red/GTK` requirements)

1. Install 32-bit dependencies: `at-spi2-core, libgtk-3-bin`
1. Download code: `git clone -b GTK https://github.com/red/red.git`
1. Download *REBOL*: `wget http://www.rebol.com/downloads/v278/rebol-core-278-4-3.tar.gz && tar xzvf rebol-core-278-4-3.tar.gz && cp releases/rebol-core/rebol red/`
1. `cd red`
1. Edit `environment/console/CLI/console.red`, and add `Needs: View` line inside `Red []` block.
1. Compile: `echo 'Rebol[] do/args %red.r "-r %environment/console/CLI/console.red"' | ./rebol +q -s`
1. Test: `./console tests/react.test` should open a window with red/green/blue sliders that change color in a box next to them and color of text below them. More tests can be found in `tests` and `tests/gtk3` folders. You can also run console in interactive mode: `./console` and enter the line: `view [button "hello"]`.

## Developer notes

For GTK, use official GTK docs. Instead of using GTK3's recommended CSS-like API, [we try to use](https://rebol.tech/gitter.im/red/GTK/2019/#msg5c32ba4c26d86e4d5638d894) deprecated low-level API, used also by [SWT library](https://www.eclipse.org/swt/)

There is not much documentation available. You must rely on [official View documentation](https://doc.red-lang.org/en/view.html#) and `View` code – `modules/view` folder in Red's sources. GTK implementation sits in `modules/view/backends/gtk3`, with most important files: `gui.reds`, `gtk.reds`, `events.reds` and `handlers.reds`.

## Links

* @rcqls' private notes about Gtk development, may be outdated: https://cqls.dyndoc.fr/RedGtk
* Docker image that can be used to simulate 32-bit environment: https://github.com/rcqls/docker-red-gtk
* Last generated `console-view` from `rcqls/red:GTK-dev` branch: 
  * with camera stuff: https://cqls.dyndoc.fr/users/RCqls/Red/console-view
  * without camera stuff: https://cqls.dyndoc.fr/users/RCqls/Red/console-nocam
* What is done and what is missing: https://trello.com/c/aoO1zUGr/156-gtk3-gui-backend

## Notes for experimental camera stuff

Last red binary https://cqls.dyndoc.fr/users/RCqls/Red/red-gtk and https://cqls.dyndoc.fr/users/RCqls/Red/console-view include preliminary camera stuff that require some further dependencies. 

Let us notice that, from now, only one camera is detected at the device /dev/video0.

* Inside .bah_profile (or equivalent):
```
export RED_GTK_CAMERA=YES
export GST_V4L2_USE_LIBV4L2=1
```
* gstreamer installation (Ubuntu example)
```
apt-get install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good 
```
* optional installation (for further development)
```
apt-get install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-pulseaudio
```
* in case plugin gtksink is not already installed in standard plugins above: 
```
apt-get install gstreamer1.0-gtk3
```
* for detection of webcam
```
apt-get install  libgudev-1.0-0
```
* preliminary test to apply to check if `gstreamer` stuff is working properly
```
gst-launch-1.0 v4l2src ! videoconvert ! gtksink 
```
which launchs a camera viewer. Normally if this test passes camera is supposed to work inside red.
* If you test red/tests/view-test.red, debug mode needs to be deactivated (issue nothing related to camera).

## Known problems

* Some floating point arithmetic [give strange results](https://rebol.tech/gitter.im/red/GTK/2019/#msg5c41de8df780a1521f2de084) on GTK branch and a specific LOCALE. For example `load "0.00000152"` returns `0.0` or `5 / 2.0` returns `2,5.0`.

There is a workaround for LOCALE bug. Just use "C" locale, like this: `LC_ALL=C path/to/console-view`