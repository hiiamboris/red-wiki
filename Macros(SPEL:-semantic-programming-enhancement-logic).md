First of all, I advocate we call macros SPELs in order to get rid of the confusion when people come from other languages.  This will also allow people who want to write tutorials about such a feature to easily(or more easily) google the word SPEL over macro.  My recommendation of SPEL is not my own idea it comes from Conrad Barski: http://www.lisperati.com/no_macros.html

Shen, a dialect of lisp, will be what is used for macro examples.  I will try to explain the syntax in order for readers of this wiki to be able to absorb the idea of macros.  Come to this channel for questions, comments, approvals, disapprovals, or whatever else: https://gitter.im/red/red/lisp

We will begin(and end) by discussing two examples: max and thread.

In Shen, one defines a function at the repl like so...

![](http://ibin.co/2SfWYyyjHw9W)

If we use the repl with this function we get 

![hi there](http://ibin.co/2SfUr8mOpatn)

The numbers in the function are patterns, when a pattern matches(the number on the left) it is mapped to some output(the strings on the right)

![](http://ibin.co/2SfX7KJ0XIly) (made a mistake on the mappings from the previous definition, but it does not hurt the tutorial)
