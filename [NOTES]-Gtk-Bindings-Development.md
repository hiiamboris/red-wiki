# GTK Bindings Development

* Sources: https://github.com/rcqls/red/tree/GTK (_rcqls/red:GTK_ branch)
* Gitter channel (chat, help): [red/GTK](https://gitter.im/red/GTK)

## Compilation (32-bit Linux)

(see [rcqls/docker-red-gtk Dockerfile](https://github.com/rcqls/docker-red-gtk/blob/master/Dockerfile) )

1. Install 32-bit dependencies: `at-spi2-core, libgtk-3-bin, libcanberra-gtk3-module`
1. Download code: `git clone -b GTK https://github.com/rcqls/red.git`
1. Download *REBOL*: `wget http://www.rebol.com/downloads/v278/rebol-core-278-4-3.tar.gz && tar xzvf rebol-core-278-4-3.tar.gz && cp rebol-core/rebol red/`
1. `cd red`
1. Edit `environment/console/CLI/console.red`, and add `Needs: View` line inside `Red []` block.
1. Compile: `echo 'Rebol[] do/args %red.r "-r %environment/console/CLI/console.red"' | rebol +q -s`
1. Test: `./console tests/react.test` should open a window with red/green/blue sliders that change color in a box next to them and color of text below them. More tests can be found in `tests` and `tests/gtk3` folders

## Notes

Instead of using GTK3's recommended CSS-like API, [we try to use](https://gitter.im/red/GTK?at=5c32ba4c26d86e4d5638d894) deprecated low-level API, used also by [SWT library](https://www.eclipse.org/swt/)

*red/red:GTK* branch will be updated by Red's core developers team [whenever needed](https://gitter.im/red/GTK?at=5c3463bc1d1c2c3f9cdd2d41).

## Links

* @rcqls' private notes about Gtk development, may be outdated: https://toltex.u-ga.fr/RedGtk
* Docker image that can be used to simulate 32-bit environment: https://github.com/rcqls/docker-red-gtk
