# REP 0101 - For loop function
<table>
  <tr>
    <td>REP:</td>
    <td>0101</td>
  </tr>
  <tr>
    <td>Title:</td>
    <td>FOR loop function</td>
  </tr>
  <tr>
    <td>Author(s):</td>
    <td>Gregg Irwin</td>
  </tr>
  <tr>
    <td>Status:</td>
    <td>Submitted</td>
  </tr>
  <tr>
    <td>Date Created:</td>
    <td>30-Sep-2016</td>
  </tr>
  <tr>
    <td>Date Last Actioned:</td>
    <td>30-Sep-2016</td>
  </tr>
</table>

## Summary
A general purpose `for` loop is useful and well known (https://en.wikipedia.org/wiki/For_loop). It makes sense to include one in Red, even if there are specific, optimized loop constructs that handle subsets of its functionality.
     
## Description
http://www.rebol.com/r3/docs/guide/code-loops.html shows that Rebol has a number of loop constructs, which Red has inherited. Rebol also has a `for` function, but its use was discouraged under R2, mainly for performance reasons. It also suffered due to the large-ish number of args which were not optional or named. More importantly, it could end up in an infinite loop and the cycle test behavior was not always clear. There was extensive discussion on CureCode about how to address these issues: http://issue.cc/r3/1993

The idea is to support *mainly* numeric loop constructs. Working like `foreach` and `forskip` is also possible, but the current implementation doesn't work correctly under Red. It is also not meant to be used in place of `forever`.

*Note: I originally proposed this for Rebol under the name `loop`, because that is what it does, and it was designed to be interface compatible with Rebol's `loop` func.*

Interface:

- LOOP compatible
    `for 5 [print "x"]`
- REPEAT compatible (default start value of 1)
    `for [i 5] [print i]`
- C-like (per Ladislav's CFOR function)
    `for [ [i: 1] [i <= 1000] [i: i + i] ] [print i]`
- Rebol FOR compatible (except for all args being in a block)
    `for [i 5 10 2] [print i]`
- Like FOR, but with a default bump of 1
    `for [i 5 10] [print i]`
- Or -1 if start is greater than end
    `for [i 10 5] [print i]`
- Optional key words can be supported in the dialect, to aid readability and make new users more comfortable
```red
	for [i = 5 to 10 step 2] [print i]
	for [i = 5 to 10 by 2] [print i]
	for [i = 5 to 10 step by 2] [print i]
```

## Use Case

The use case of a ```for``` function is to provide a way of looping over data in a way that is common in procedural programming languages. 

Examples of how it would be used are:

```red
a: 1
b: 5

    [for 5 [print "x"]]
    x
    x
    x
    x
    x
    [for [ [i: 1] [i <= 1000] [i: i + i] ] [print i]]
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

## Benefits
- General, subsuming many other loop constructs
- Flexible, with optional args and complete control over the loop cycle
- Shows how dialects can be built to wrap other functionality
- Delegating to native loop constructs when possible keeps performance high
- Single starting point for looping, like `round` is for rounding
- New users find a `for` function that should "just work"
- Can't accidentally loop infinitely (though the C-like interface makes it possible)
- See http://issue.cc/r3/1993 for details on the design goals
- Easy to support date!, time!, and money! values when those are available

## Consequences
- Reduces Rebol compatibility (close, but args are passed in a block)
- May result in requests for more complexity in current function interfaces 
- Could lead to lengthy debate on the dialect other design aspects
- Hard to provide detailed dialect help in a short doc string

## Assistance
I have an implementation and manual test suite to get the ball rolling.

## Community Support
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

## Debate
The purpose of this section is to allow members of the community to succinctly express either (or both) the pros and cons of the proposal. Links to supporting information should be included.

This is not the place for long, discussion related to the proposal. The best place for such discussions would be the [Red Mailing List](https://groups.google.com/forum/#!forum/red-lang) as the conversations can be linked to from the proposal. Such discussions can also be held on [Red Gitter Chat](https://rebol.tech/gitter.im/red/red) though they will not be preserved in such a convenient form as the mailing list.

 __This section will be curated by the Red Team.__

Author: \<The real name of the author.\>

Date: \<The date added to the proposal.\>

Point: \<The succinct reasons in support of or against the proposal.\>

Author: Arnold van Hofwegen

Date: 30 sep 2016

Point: First I want to express the fact that I appreciate the work and thought Gregg has put into this proposal. Nevertheless I consider adding the FOR loop to be a bad addition. As mentioned there are plenty good alternatives available that do the job and invite the (new) user to come closer to the language and its proper use. FOR would be (mis)used in almost all cases where the existing alternatives should be used. I have the feeling the wish for FOR is given in by the wish to please developers coming from C / Java / name-it background. While it is good to have good adaptation for Red, this is not the way to achieve that. You might as well introduce IF/THEN/ELSE and remove EITHER next. Pleasing others is opening the way for complexity and bloatware mindedness again. I believe the adaption of Red should come from its own strengths, the "killer" apps we will make as soon as even more functionality will be embedded in the language. 
Sorry, for the lengthy posting.