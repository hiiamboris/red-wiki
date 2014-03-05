There are many ways to contribute to Red in addition to making [donations](http://www.red-lang.org/p/donations.html). This is a simple "how to" guide.

There are seven different ways to contribute to the Red project:
* Make fixes and enhancements to the Red and Red/System core and runtimes
* Write and maintain Red mezzanine functions, modules, objects and schemes   
* Write and maintain Red library functions
* Write and maintain Red/System library functions
* Write and maintain documentation and documentation systems
* Write and maintain Red and Red/System tests
* Use Red and Red/System and submit meaningful bug reports

No matter how small, your contribution will be valued and appreciated providing that you follow the guidelines. In particular, isolating bugs so that they can be easily identified and fixed is a great help.

### General Contribution Guidelines
1. You should be sure that you own the Intellectual Property Rights(IP) of any contribution you make to the Red project.
2. By submitting a contribution to the Red project, you are granting the Red project and, in particular, Nenad Rakocevic the right to publish your submission.
(A simple way for you to confirm both 1 & 2 will be introduced in due course.)
3. A lot of care and attention has been given to the design of both the Red and Red-System languages. Before starting work on anything to change or extend either language, please submit a proposal for your change by raising an issue on the Red project GitHub repository. (It could save you a lot of unnecessary work.)
4. All code submissions should include a reasonable set of tests written with [quick test](http://static.red-lang.org/red-system-quick-test.html).

### Coding Standards
All contributions should adhere to the following coding standards

1. All source code should be UTF-8 encoded.
2. All indents should be made using tabs not spaces and be 4 characters wide.
3. Function specifications should follow a vertical layout:

```
my-func: func [
    arg1   [datatype!]
    arg2   [datatype!]
    /local
        local1    [datatype!]
        local2    [datatype!]     
][
…
]
```

4. End-of-line comments are preferred over between line comments. The should be preceded with ";—-" starting at position 57.

```
    my-result: my-fantastic-func a b                    ;— my very clever comment
``` 


### Test Standards
Every code-based contribution should be accompanied by a meaningful set of tests. Tests should be written using the quick-test.red or quick-test.reds frameworks. Tests requiring checking console output from your code or compiler message should be written using quick-test.r. If you are unfamiliar with quick-test, check out the [documentation](http://static.red-lang.org/red-system-quick-test.html).

The following approach to writing tests should be used:

1. A separate test file should be used for each functional unit included in your code that you submit.
2. Tests should be grouped by functionality.
3. Each test should be independent from all other tests. The results of a test should not be dependent upon the results of any other test. (It should be possible to remove any test from a file and the other tests should still all pass).
4. Each test should have a unique "name" so that it can be quickly found by searching the test file.
5. The project coding standards should be followed.
6. For short tests both the test header and the assert should be written on a single line.
7. With longer multi-line tests, the assert should be indented from the test header.
8. The following indentation scheme should be used:
```
Red [ … ]
#include %<path-to>/quick-test/quick-test.red

~~~start-file~~~ "my-contribution"

        startup code

===start-group=== "my-cont-func-str"

        group start up code

    --test-- "mcf-str-1" --assert "yes" = my-cont-func "A string"

    --test-- "mcf-str-2"
        mcf-str-2-str: "A string"
        --assert "yes" = my-cont-func mcf-str-2-str

       group tidy up code

===end-group===

===start-group=== "my-cont-func-block"

        group start up code

    --test-- "mcf-blk-1" --assert "no" = my-cont-func [a b c d]

    --test-- "mcf-blk-2"
        mcf-blk-2-blk: [a b c d]
        --assert "no" = my-cont-func mcf-blk-2-blk

       group tidy up code

===end-group===

       file tidy up code

~~~end-file~~~

```
9. Wherever possible large volume tests should be generated to reduce the work required to maintain the tests. Currently test generators should be written in Rebol 2 and should be capable of being run under Rebol Core 2.7.8. At some point in the future, all test generators will have to be ported to Red. Test generators need to provide a mechanism where by they automatically generate a revised set of tests if the generator has been updated. Please study the mechanisms used in [Red run-all.r](https://github.com/red/red/blob/master/tests/run-all.r), [Red/System run-all.r](https://github.com/red/red/blob/master/system/tests/run-all.r), [Red make-equal-auto-test.r](https://github.com/red/red/blob/master/tests/source/units/make-equal-auto-test.r), [Red make-interpreter-auto-test.r](https://github.com/red/red/blob/master/tests/source/units/make-interpreter-auto-test.r) and [Red/System make-dylib-auto-test.r](https://github.com/red/red/blob/master/system/tests/source/units/make-dylib-auto-test.r). 

### Red and Red/System core and runtimes

### Red mezzanine functions, modules, objects and schemes
In the Red project, mezzanine code refers to functions, modules, objects and schemes written in Red that are included in the Red binary. Library code refers to functions, etc. that are included in the repository but not distributed in the binary versions of Red. They must be specifically included for use in Red programs. (There is not currently a mechanism to do this … but it shouldn't be too long before there is.)

The process for code to be included as Red mezzanine is as follows:

1. Submit a pull request of the code for inclusion the Red library.
2. Once the code is included in the Red library, submit a proposal for its inclusion within the Red mezzanine code via a Red project GitHub issue.
3. If the proposal is accepted by the Red project team, submit a pull request with your code included as mezzanine code and a revised set of tests.

At the current stage of Red's development, mezzanine code is not yet being accepted so please do not submit proposals until they are.

### Red library functions
### Red/System library functions
### Documentation and documentation systems
### Red and Red/System tests
### Bug reports