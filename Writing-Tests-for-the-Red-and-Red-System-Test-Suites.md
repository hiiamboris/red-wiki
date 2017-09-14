The Red Language currently has two test suites, one for Red, the other for Red/System. The tests are written and run using the [Red Quick Test Framework](http://static.red-lang.org/red-system-quick-test.html). Quick test is a minimal framework that consists of two dialects, one for writing tests, the other for running tests. The dialect for writing tests is implemented for Red, Red/System and Rebol. The dialect for running tests is written in Rebol.
(At some stage before Red 1.0, quick test will be implemented solely in Red).

The implementations of the test writing dialect are:

*quick-test.red for testing Red code
*quick-test.reds for testing Red/System code
*quick-unit-test.r for testing Rebol code

The implementation of the test running dialect is in quick-test.r

## The Tests

The vast majority of tests are written using quick-test.red and quick-test.reds. There are very few Rebol unit tests. A few tests of compiler features are written using quick-test.r as each test requires separate compilation. 

This remainder of this document currently relates only to tests written using quick-test.red and quick-test.reds.

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