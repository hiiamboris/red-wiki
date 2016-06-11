#REP XXXX - \<Dialected FOR function\>
<table>
  <tr>
    <td>REP:</td>
    <td>&lt;Leave Blank&gt;</td>
  </tr>
  <tr>
    <td>Title:</td>
    <td>&lt;Dialected FOR function&gt;</td>
  </tr>
  <tr>
    <td>Author(s):</td>
    <td>&lt;Gregg Irwin(s)&gt;</td>
  </tr>
  <tr>
    <td>Status:</td>
    <td>Draft</td>
  </tr>
  <tr>
    <td>Date Created:</td>
    <td>&lt;Leave blank&gt;</td>
  </tr>
  <tr>
    <td>Date Last Actioned:</td>
    <td>&lt;Leave blank&gt;</td>
  </tr>
</table>

##Summary
A general purpose `for` loop is useful and well known (https://en.wikipedia.org/wiki/For_loop), so it makes sense to include it in Red, even if there are specific, optimized loop constructs that handle subsets of its functionality.
     
##Description
http://www.rebol.com/r3/docs/guide/code-loops.html shows that Rebol has a number of loop constructs, which Red has inherited. Rebol also has a `for` function, but its use was discouraged under R2, mainly for performance reasons. It also suffered due to the large-ish number of args which were not optional or named. More importantly, it could end up in an infinite loop and the cycle test behavior was not always clear. There was extensive discussion on CureCode about how to address these issues: http://issue.cc/r3/1993


##Use Case

```
[for 5 [print "x"]]
x
x
x
x
x
[for [[i: 1] [i <= 1000] [i: i + i]] [print i]]
1
2
4
8
16
32
64
128
256
512
[for [i 5 10 2] [print i]]
5
7
9
[for [i 10 5 -1] [print i]]
10
9
8
7
6
5
[for [i 5 10] [print i]]
5
6
7
8
9
10
[for [i 5] [print i]]
1
2
3
4
5
[for [i 1000000] []]
[for [i -2 -5] [print i]]
-2
-3
-4
-5
[for [i -2] [print i]]
1
0
-1
-2
[for [i 0 1 0.2] [print i]]
0
0.2
0.4
0.6
0.8
1.0
[for [i 1 2 1] [print i]]
1
2
[for [i 2 1 -1] [print i]]
2
1
[for [i 2 1 1] [print i]]
[for [i 1 2 -1] [print i]]
[for [i 1 2 0] [print i]]
[for [i 2 1 0] [print i]]
[for [i 1 1 1] [print i]]
1
[for [i 1 1 -1] [print i]]
1
[n: 0 
    for [i 1 1 0] [print i n: n + 1 if n > 2 [print '... break]]
]
[for [i 5 5 1] [print i i: -5]]
5
[for [i 5 5 1] [print i i: 3]]
5
[
    n: 0 
    for [i 5 5 1] [print i i: 4 n: n + 1 if n > 2 [print '... break]]
]
5
5
5
...
[for [i 5 5 1] [print i i: 5]]
5
[for [i 5 5 1] [print i i: 6]]
5
[for [i 5 5 -1] [print i i: 3]]
5
[for [i 5 5 -1] [print i i: 4]]
5
[for [i 5 5 -1] [print i i: 5]]
5
[
    n: 0 
    for [i 5 5 -1] [print i i: 6 n: n + 1 if n > 2 [print '... break]]
]
5
5
5
...
[for [i 5 5 -1] [print i i: 7]]
5
[for [i a b 1] [print i]]
1
2
3
4
5
[for [i ser-a ser-b 1] [print mold i]]
[a b c d e f g]
[if error? 
    try [for [i ser-a b 1] [print mold i]] [print #**ERR]
]
**ERR
[for [i ser-a skip ser-a 5 2] [print mold i]]
[a b c d e f g]
[for [i ser skip ser 5 2] [print mold i]]
[1 2 3 4 5 6 7]
[for [i ser] [print mold i]]
1
2
3
4
5
6
7
[if error? 
    try [for [i #"û" #"ÿ"] [print mold i]] [print #**ERR]
]
**ERR
[for [i 1.0e30 1.0e40 1.0] [print i i: i * 10.0]]
1.0e30
1.0e31
1.0e32
1.0e33
1.0e34
1.0e35
1.0e36
9.999999999999998e36
9.999999999999998e37
9.999999999999998e38
9.999999999999998e39
[for [i = 5 to 10 step 2] [print i]]
5
7
9
[for [i = 5 to 10 by 2] [print i]]
5
7
9
[for [i = 5 to 10 step-by 2] [print i]]
5
7
9
[for [i = 5 to 10 step by 2] [print i]]
5
7
9
[for [i = 5 10 2] [print i]]
5
7
9
[for [i 5 to 10 2] [print i]]
5
7
9
[for [i 5 10 step 2] [print i]]
5
7
9
```

##Benefits
- Universal, subsuming many other loop constructs
- Flexible, with optional args and complete control over the loop cycle
- Shows how dialects can be built to wrap other functionality
- Delegating to native loop constructs when possible keeps performance high
- Single starting point for looping, like `round` is for rounding
- New users find a `for` function that should "just work"

##Consequences
- Not Rebol compatible
- More complex interface
- Complex implementation
- Could lead to lengthy debate on the dialect other design aspects

##Assistance
I have an implementation and small manual test suite to get the ball rolling.

##Community Support
__Do not complete until initial draft has been accepted.__

The following people support the proposal in its entirety. 
<table>
  <tr>
    <th>Real Name</th>
    <th>Github Account</th>
  </tr>
  <tr>
    <td>&lt;Real Name&gt;</td>
    <td>&lt;Link to Github Account&gt;</td>
  </tr>
</table>

##Debate
The purpose of this section is to allow members of the community to succinctly express either (or both) the pros and cons of the proposal. Links to supporting information should be included.

This is not the place for long, discussion related to the proposal. The best place for such discussions would be the [Red Mailing List](https://groups.google.com/forum/#!forum/red-lang) as the conversations can be linked to from the proposal. Such discussions can also be held on [Red Gitter Chat](https://gitter.im/red/red) though they will not be preserved in such a convenient form as the mailing list.

 __This section will be curated by the Red Team.__

Author: \<The real name of the author.\>

Date: \<The date added to the proposal.\>

Point: \<The succinct reasons in support of or against the proposal.\>
