# Clearing %libRedRT when using `-o` on the command line

Normally you can just do `rebol -qs red.r clear`, which clears %libRedRT in the working folder. If you use `-o` to specify an alternate output dir, e.g., `rebol -qs red.r -c -o ../../Code/Red/test ../../Code/Red/test.red`, you need to add a path argument to `red clear`, because %libRedRT is built in the same dir as the executable. Otherwise the exe couldn't find it.

