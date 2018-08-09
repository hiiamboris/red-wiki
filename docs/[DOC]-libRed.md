# Call libRed from C 

`%red-in-c.c`
```c
#include "<path to>/libRed/red.h"

int main(int argc, const char * argv[]) {
    redOpen();
    redDo("print {Hello Red}");
    redClose();
    return 0;
}
```

## macOS
Here's the command to compile it on macOS (As libRed is 32-bit, you must use gcc to compile the program so it can call libRed) :

```shell
$ gcc -m32 <path to>/libRed.dylib -o red-in-c red-in-c.c
```

## ubuntu32

```shell
gcc red-in-c.c -L ./ -l Red -I <path to>/libRed -o red-in-c
```

## Windows using the Tiny C Compiler

```shell
tcc red-in-c.c -L <path-to-dir-containing-libRed.dll>\ -lRed -o red-in-c.exe
```

# Call libRed dynamically from C (not linking to libRed)

## macOS & Ubuntu32

### Code
```c
#include <stdlib.h>
#include <stdio.h>
#include <dlfcn.h>

int main(int argc, const char *argv[]) {
    
    void *handle;
    char *error;
    void (*redOpen)();
    void (*redDoFile)(char *);
    void (*redClose)();
    
    handle = dlopen("<absolute-path-to>/libRed.<dylib|so>", RTLD_LAZY);
    if (!handle) {
        fputs(dlerror(), stderr);
        exit(1);
    }
    redOpen = dlsym(handle, "redOpen");
    if ((error = dlerror()) != NULL) {
        fputs(error, stderr);
        exit(1);
    } 
    redDoFile = dlsym(handle, "redDoFile");
    if ((error = dlerror()) != NULL) {
        fputs(error, stderr);
        exit(1);
    } 
    redClose = dlsym(handle, "redClose");
    if ((error = dlerror()) != NULL) {
        fputs(error, stderr);
        exit(1);
    } 
    
    (*redOpen)();
    (*redDoFile)("<path-to>/<file>.red");
    (*redClose)();
    
    return(0);
}
```
### Compilation (gcc)
```text
gcc -m32 -o <executable-filename> <source-file>.c -ldl

```
# Call libRed from Ruby (Ubuntu32) using the FFI Gem

```ruby
require 'ffi'

module RubyRed
  extend FFI::Library
  ffi_lib "./libRed.so"
  attach_function :redOpen, [], :void
  attach_function :redClose, [], :void
  attach_function :redDo, [ :string ], :pointer
end

RubyRed::redOpen
RubyRed::redDo "print {Hello Ruby, I'm Red}"
RubyRed::redClose
```

# .NET CS binding (@koba-yu)

https://github.com/koba-yu/LibRedSharp

# Testing libRed from LINQPad (@dander)

https://gist.github.com/dander/a02c8837e12e70ec24396da3a18b1168

# Why is LIBRed 32-bit only?

Red is still in the alpha stage (as of early 2017). 64-bit support will come, but it will probably happen after 1.0 because of the effort needed. It will come, though, because we need to be able to call libRed from 64-bit tools and language implementations. For 64-bit support, Red needs:

1. A 64-bit emitter for x64 and ARM64.
2. Support for 64-bit integers and pointers in Red/System.
3. Changes to the value slots, which are 128 bits currently, to cope with 64-bit pointers.
4. 64-bit executable file format support.

1 & 4 would be trashed once we start working on Red 2.0, so it's not worth investing time in that right now. 2 could be partially implemented before 1.0. 3 requires significant changes in the runtime code, and an elegant solution to handle both 32 and 64-bit models within the same codebase.