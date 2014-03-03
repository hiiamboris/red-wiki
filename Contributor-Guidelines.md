There are many ways to contribute to Red in addition to making [donations](http://www.red-lang.org/p/donations.html). This is a simple "how to" guide.

There are seven different ways to contribute to the Red project:
* Make fixes and enhancements to the Red and Red/System core and runtimes
* Write and maintain Red mezzanine functions, modules, objects and schemes   
* Write and maintain Red library functions
* Write and maintain Red/System library functions
* Write and maintain documentation and documentation systems
* Write and maintain Red and Red/System tests
* Use Red and Red/System and submit meaningful bug reports

No matter how small, you contribution will be valued and appreciated providing that you follow the guidelines. In particular, isolating bugs so that they can be easily identified and fixed is a great help.

### General Contribution Guidelines
1. You should be sure that you own the Intellectual Property Rights(IP) of any contribution you make to the Red project.
2. By submitting a contribution to the Red project, you are granting the Red project and, in particular, Nenad Rakocevic the right to publish your submission.
(A simple way for you to confirm both 1 & 2 will be introduced in due course.)
3. A lot of care and attention has been given to the design of both the Red and Red-System languages. Before starting work on anything to change or extend either language, please submit a proposal for your change by raising an issue on the Red project GitHub repository. (It could save you a lot of unnecessary work.)
4. All code submissions should include a reasonable set of tests written with [quick test](http://static.red-lang.org/red-system-quick-test.html).

### Coding Standards
All contributions should adhere to the following coding standards

1. All source code should be UTF-8 encoded.
2. All indents should be made using tabs not spaces.
3. Function specifications should follow a vertical layout, e.g.

`my-func: func [
     arg1     [datatype!]
     arg2     [datatype!]
     /local
         local1   [datatype!]
         local2   [datatype!]
][
…
]`
    
4. End-of-line comments are preferred over between lines comments. The should be preceded with ";—-" starting at position 57.

### Quick Test Standards

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