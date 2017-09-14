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

### Test Location
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

WIP
Nenad likes the --test-- line to stand out in the tests so that he can spot them quickly. So we use two formats - concise tests (which fit on one line) and standard tests.
The basic format for a concise test is
<one tab>--test-- "test-name"<a few tabs>--assert
Concise tests are written without a new line behind them.

The basic format for a standard test is:
<empty line>
<one tab>--test-- "test-name"
<two tabs><test code>
<two tabs><--assert>
<two tabs><more test code>
<two tabs><--assert>
<empty line>
I try to keep all the tests completely independent so avoid sharing set up code. I want to avoid the risk of one test possibly affecting another. In my own code I even go to the extent of not reusing words used in other tests. (Perhaps this is verging on paranoia about tests falsely passing because of the content of a value set by a previous test)