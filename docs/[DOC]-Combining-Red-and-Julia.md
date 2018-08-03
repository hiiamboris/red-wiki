There are different ways how to combine Red and Julia programs.  They are explored here.

# From Red side

## Calling Julia as a command

You can use `call` in Red to integrate with Julia as well.  The `/input` refinement of `call` is useful to input Julia code. 

## Embedding Julia Code in Red (FFI)

Bindings for embedding `libjulia` into a Red program have already been contributed.

https://gist.github.com/Oldes/61444afa55a0395fde60002a5b345518

> The above gist includes a path to Julia that you'll need to change.

You may need to copy the binary to the same path as the bin folder in julia

Unfortunately, Red is currently 32-bit only, so you can't use it with 64-bit Julia.  It is possible to use 32-bit Julia now, or to wait till Red is 64-bit as well.

### References

How to embed Julia (Julia documentation):
https://docs.julialang.org/en/release-0.5/manual/embedding/


# From Julia

## Embedding Red Code in Julia (FFI)

A Julia package to call Red from Julia https://github.com/joa-quim/Red.jl
Calling graphical functions works fine.

### Example: Calling a Julia function from a Red GUI defined in Julia

```julia
using Red

function blabla()
    println("Blabla")
    return redInteger(0)
end

cf = cfunction(blabla, Ptr{Void}, (),);
redOpen()
redRoutine(redWord("blabla"), "[]", cf);

cmd = "view [backdrop yellow  t: text  yellow {Hello World !}
    button {Click} [blabla] 
]"

redDo(cmd)
```

### Example: Establish communication between Red and Julia.

Julia callback updates text in Red Window.

```julia
using Red

function blabla(t)
    face_text = redPath(redWord("t"), redWord("text"))
    println(unsafe_string(redCString(redGetPath(face_text))))

    id = redSymbol("text");
    redSetField(t, id, redString("BlaBla"))
    redProbe(redGetField(t, id));
    return redUnset()
end

cf = cfunction(blabla, Ptr{Void}, (Ptr{Void},));
redOpen()
redRoutine(redWord("blabla"), "[obj [object!]]", cf);

cmd = "view [backdrop yellow  t: text  yellow {Hello World!}
    button {Click} [blabla t] 
]"
redDo(cmd)
```

# Two way communication
## Sockets

Waiting for full I/O support in Red.