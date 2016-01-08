First of all, I advocate we call macros SPELs in order to get rid of the confusion when people come from other languages.  This will also allow people who want to write tutorials about such a feature to easily(or more easily) google the word SPEL over macro.  My recommendation of SPEL is not my own idea it comes from Conrad Barski: http://www.lisperati.com/no_macros.html

Shen, a dialect of lisp, will be what is used for macro examples.  I will try to explain the syntax in order for readers of this wiki to be able to absorb the idea of macros.  Come to this channel for questions, comments, approvals, disapprovals, or whatever else: https://gitter.im/red/red/lisp

We will begin(and end) by discussing two examples: max and thread.

In Shen, one defines a function at the repl like so...

![](http://ibin.co/2SfWYyyjHw9W)

If we use the repl with this function we get 

![hi there](http://ibin.co/2SfUr8mOpatn)

The numbers in the function are patterns, when a pattern matches(the number on the left) it is mapped to some output(the strings on the right)

![](http://ibin.co/2SfX7KJ0XIly) (made a mistake on the mappings from the previous definition, but it does not hurt the tutorial)

Unlike other lisps, Shen uses block syntax to denote the quoted list:

'(1 2 3) => other lisps

[1 2 3] => Shen

variables are represented with capital letters.

Knowing this, let us write a couple more functions in order to see some more examples of pattern matching:

![](http://ibin.co/2SfrW7c8ud6z)

A vertical bar | means to place the X variable onto the list XS.  It is actually a shortcut for consing (see https://en.wikipedia.org/wiki/Cons for more information on cons)

Basically first will only accept lists, it is the only pattern that it contains, it will break apart the list and bind the first element to X.  This is called pattern matching, it will be used in our macro example, so an elementary understanding of it is required.

Now, let us define our first function that we will invoke a macro on.  

![](http://ibin.co/2ShhumMuvHL7)

Notice that max has a fixed arity of two.  We could handle an arbitrary amount of arguments using a list, however, for performance reasons and syntactic ones we decide that a macro would be better.

If we were to use a list we would have something like this:

![](http://ibin.co/2Shlp3nrSkjj)

There is a lot of overhead here.  The pattern matching, the consing, etc.  Would it not be nice if we could have something like...

(max 1 (max 2 (max 3 (max 4))))

Where max here refers to the original definition?

It would also be nice if the syntax could be (max 1 2 3 4) instead of introducing and constructing a list.

Macros in Shen are syntactic patterns that get recognized by the reader. It is the task of the Shen reader to read in a given expression and it is the task of the Shen evaluator to evaluate it.  (+ 1 2) for example is read in as a list      [+ 1 2]. If the reader recognizes a pattern of syntax as a valid macro expansion, the pattern/macro gets expanded and then the code is evaluated.

Shen macros have two rules:

1.  Every macro takes one and only one input; i.e. it's arity is one.
2.  Every macro has a default case X -> X inserted by Shen as a final rule. Thus the macro always behaves as the identity function if the rules supplied by the programmer failed to apply to an input.

Having this knowledge, let us write our first macro.

![](http://ibin.co/2Sm7GuYrTggO)

![](http://ibin.co/2SmAUjNCulKY)

This will, of course, recurse over and over again until the reader no longer finds a pattern that matches that which was laid out in the macro.  The macro-expansion function shows us exactly what the evaluator will see when this process is done.  What is left is part of the lisp image, there is no do, there is no additional evaluation, just raw code.  We could optimize this a little further so that whenever the macro "sees" max with numbers like that, instead of symbols, it will go ahead and fully calculate everything, leaving a single number behind as the result:

![](http://ibin.co/2SmJe1z85Cf7)

Notice that when max is given numbers the result is a single number calculated ahead of time, that will be the only evaluation at run time, a number.  Notice that when given symbols max intelligently expands as it should so that it can still be used with some abstraction.  Once again, lisp will "see" (max A (max B C)) instead of (max 1 2 3 4) or any code associated with its reorganization.  Notice also that max is still just a function, it can be passed as an argument to other functions.  The macros in shen act on syntactic patterns, we chose max arbitrarily, max-macro could recognize whatever pattern we deem fit.

How would we do this in red?

There are two thoughts that I have had, one is to do macros like common lisp, the other is to do macros like Shen.  We might be able to combine the two approaches.  We cannot simply adopt Shen's style as is because the free from syntax of red will not allow it.  For example:

Say we have a macro system like this:

Create a pattern in some spel, where the pattern/code we are attempting to match: max 1 2 3

if the pattern matches...

1. gather the code into a block
2. dispatch code for that pattern.
3. drop the outermost block if there is one

Here is a simple idea of implementation:

rule-for-max:   [max 3 digit]

dispatch-for-max: [loop length? remove at 1 code [insert code 'max]] 

spel max-pattern rule-for-max dispatch-for-max

max 1 2 3 => pattern matches!

gather code into a block, call that block code so that the dispatch can refer to it.

code: [max 1 2 3]

apply dispatch-for-max

code: [max max 1 2 3]

drop the block

max max 1 2 3


Although this would seem nice at first, if one thinks about it, there are many problems here.
After the manipulation, we still have max 1 2 3 which matches the pattern of the spel.  Would this recurse forever?  

Maybe a single pass spel system should be adopted where the reader can just move on after a single expansion.  There is still problems with such an approach.

say, for whatever reason you forgot how your macro worked and you wrote: max max 1 2 3
Now, the expander will return max max max 1 2 3 which is clearly wrong!  And, what if your spel spit out other spels, these would not be expanded if the reader just moved on.  

The fact is, I do not think we can marry Shen macros to such a free form system(if the red users have an idea of a fix to this please discuss such an idea in https://gitter.im/red/red/lisp) so we must reintroduce the named macro system from lisp tradition...

Current Proposal:

1. Spels are named.
2. Spels take a single block, their arity is one
3. Whatever spels do at expansion they must return a block.
4. The block that is returned, is removed. ex: [rebol code [returned from] a macro] => rebol code [returned from] a macro

max would now work something like this...

max-helper: func [x y] [either x > y [x][y]]
spel max [loop length? code [insert code 'max-helper]]

max [1 2 3] =>
 
[loop length? code [insert code 'max-helper]] =>

code: [max-helper max-helper 1 2 3] => macro pops block and we get

max-helper max-helper 1 2 3





   

