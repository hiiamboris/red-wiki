Helpful hints if you're just starting out.

See http://www.red-lang.org/p/download.html if you haven't already.

Questions to answer:

- Where should I put the red executable?
- Do I need to change PATH settings?
- Can I rename the executable (e.g. to red.exe from red-063.exe)
- What's the best practice to organize red files?
- Does red install some files the first time it runs? Where is this location?

Ideas:

Should we provide a recommendation for where to drop Red, so if people follow that suggestion, we can more easily troubleshoot permission issues and such? e.g. various Windows versions have rules that affect system directories, which we should note if possible.


# Windows

## Does red install some files the first time it runs? Where is this location?

Example from 2018-04-06. Running `red-06apr18-f53f9073.exe` creates these files:
* `C:\ProgramData\Red\gui-console-2018-4-6-49774.exe`
* `C:\ProgramData\Red\crush-2018-4-6-49774.dll`

If the Options/settings... in console is altered a config file is created:
* `C:\Users\UserName\AppData\Roaming\Red\Red-Console\console-cfg.red`

# MacOS

## Catalina

Until Red is natively 64-bit, you need to use a container or other solution.

- https://github.com/rebolek/red-tools/wiki/Red-on-Catalina

# Linux

# Android

# RasPi

