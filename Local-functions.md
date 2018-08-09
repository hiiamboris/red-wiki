It is possible to define a function in Red/System V1 though it is not mentioned in the Language Specification.

The following Red/System program compiles and produces the expected output:

```red
Red/System []

f: func[] [
	g: func[] [print ["g inside f" lf]]
	g
]

f
```

A call to a locally defined function from the global scope gives an undefined symbol error.

However the current implementation does not allow a local function to be assigned to a variable which has the same name as a global variable to which a function has been assigned. Declaring a local function with the same name as a global variable does not give a compiler error but doesn't give expected results). These two code examples show this more clearly than my explanation.

Code
```red
Red/System []

f: func[] [
	g: func[] [print ["g inside f" lf]]
	g
]
g: func[] [print ["g in global scope" lf]]
f
g
```

Output
```red
*** Compilation Error: attempt to redefine existing function name: g 
*** in file: %pathto/test.reds 
*** in function: f
*** at line: 4 
*** near: [
    g: func [] [print ["g inside f" lf]] 
    g
]
```

Code
```red
Red/System []

f: func[] [
	g: func[] [print ["g inside f" lf]]
	g
]
g: 1
print ["before calling f" lf]
f
print ["after calling f" lf]
```

Output
```red
...compilation time:     125 ms
...linking time:         10 ms
...output file size:     16384 bytes
...output file name:     builds/test
before calling f
after calling f
```

## Usefulness
Being able to define local functions is useful for the following reasons:

1. Giving the ability to include (and hence define) Red/System functions within a Red routine!
2. Not having to expose implementation details in Red/System libraries
3. Enable more easily readable source code as sub-routines can be defined in a function not elsewhere in the source

## Additional Syntax
It would be helpful if a variable for a local function could be defined in /Local 
