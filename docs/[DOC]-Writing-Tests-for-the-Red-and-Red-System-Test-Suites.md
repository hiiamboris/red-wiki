The Red Language currently has two test suites, one for Red, the other for Red/System. The tests are written and run using the [Red Quick Test Framework](http://static.red-lang.org/red-system-quick-test.html). Quick test is a minimal framework that consists of two dialects, one for writing tests, the other for running tests. The dialect for writing tests is implemented for Red, Red/System and Rebol. The dialect for running tests is written in Rebol.
(At some stage before Red 1.0, quick test will be implemented solely in Red).

The implementations of the test writing dialect are:

* quick-test.red for testing Red code
* quick-test.reds for testing Red/System code
* quick-unit-test.r for testing Rebol code

The implementation of the test running dialect is in quick-test.r

## The Tests

The vast majority of tests are written using quick-test.red and quick-test.reds. There are very few Rebol unit tests. A few tests of compiler features are written using quick-test.r as each test requires separate compilation. 

The remainder of this document currently relates only to tests written using quick-test.red and quick-test.reds.

### Test Locations
    red/
        tests/
              source/
                    compiler/                                Red compiler tests
                    environment/                             Red environment tests
                    library/                                 deprecated
                    runtime/                                 Red runtime tests
                    units/                                   Red language tests
        system/
               tests/
                    source/
                           compiler/                         Red/System compiler tests
                           runtime/                          Red/System runtime tests
                           units/                            Red/System dialect tests


The tests are stored in sub-directories of the red/tests/ and red/system/tests/ directories

### Writing Tests
1. A test file should contain tests for either a single datatype or a single function. There are a few exceptions to this rule where functions have very similar roles.
1. A test file should include:
    1. A Red Header
    1. A #include of quick-test.red or quick-test.reds as appropriate
    1. \~\~\~start-file\~\~\~ "\<test file name\>"
    1. one or more ===start-group=== "\<group name>\"
    1. single or multiple line tests
    1. ===end-group===
    1. \~\~\~end-file\~\~\~
    1. see [bitset-test.red](https://github.com/red/red/blob/master/tests/source/units/bitset-test.red) for an example
1. The test files should follow the [Red Coding Guidelines](https://github.com/red/red/wiki/Coding-Style-Guide).
1. Each test should be independent of all other tests.
1. Tests should be given a unique name so that they can be easily identified (by searching the test source).
1. Tests should be written to work both when compiled and interpreted.
1. Tests should be written in either concise (single line) or standard (multi line) format (see below).

#### Test Format
The --test-- line should stand out in the tests so that they can be seen at a glance.

The basic format for a concise test is:
```text
    <one tab>--test-- "test-name"<one or more tabs>--assert
```
Concise tests are written without a new line between them.

Example of concise tests:
```text
    --test-- "basic-1"	--assert "make bitset! #{00}" = mold make bitset! 1
    --test-- "basic-2"	--assert "make bitset! #{00}" = mold charset ""
    --test-- "basic-3"	--assert "make bitset! #{00}" = mold charset []
```

The basic format for a standard test is:
```text
    <empty line>
    <one tab>--test-- "test-name"
    <two tabs><test code>
    <two tabs><--assert>
    <two tabs><more test code>
    <two tabs><--assert>
    <empty line>
```
Example of standard tests:
```text
    --test-- "basic-14"
        bs: make bitset! [255 256]
        --assert 264 = length? bs
        --assert true = pick bs 255
        --assert true = pick bs 256
		
    --test-- "basic-15"
        bs: make bitset! [00010000h]
        --assert 65544 = length? bs
        --assert true = pick bs 00010000h
```

## Questions?
Please ask any questions in the [Red Gitter Tests Room](https://rebol.tech/gitter.im/red/tests).