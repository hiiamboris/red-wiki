* [Dialects of different programming paradigms]
* Red/Script that acts like Python-Django, Ruby-Rails, PHP and Perl
    * Don't you mean you want a Web-App library/framework (what does a dialect mean to you ?)
* Red/Lisp that acts like Clojure, Haskell, Erlang and Scala
    * Please specify how exactly you think Clojure, Haskell, Erlang and Scala "act" / what they or their approaches have in common and how would you extract it into dialect ?
* [dialects of different internet languages]
* Having a language that works with or replaces HTML, CSS and XML families (see W3C Recommendations)
    * Dialects for HTML ands CSS should be relatively simple . But are they necessary in (especially pre 1.0) core ?
* Having a language that works with or replaces JS, SQL, NoSQL-DB and other languages (see ISO standards) 
    * Red already is a nice dialect for Replacing JS (if someday JS gets added as compilation target).
    * Dialect for SQL generation seems _TRIVIAL_ (write it as learning experience; doesn't IMO belong in core, rather into some SQL DB interfacing lib).
    * But what exactly is "NoSQL dialect" supposed to do
    * And what should a "dialect for any other language language" accomplish ?
* https://en.wikipedia.org/wiki/Solution_stack
    * You want libraries, not dialects
* https://en.wikipedia.org/wiki/List_of_Apache–MySQL–PHP_packages
    * ditto
* [Dialects for Easter Eggs]
* Allow a golfing-specific script Red/Golf as an "Easter Egg" (similar to Sclipting, Golfscript/Flogscript, CJam, APL/J/K, Pyth, Microscript, Owk, Retina, Fourier...)
    * Any dialect is "allowed" (write it yourself), not every makes sense in the core
    * http://rebmu.hostilefork.com/ has done an example, but the Easter Egg would make it more powerful.
* Allow a brainfuck-like script Red/Brain as an "Easter Egg"
    * Classical 8-symbol BrainFuck commands (shift pointer, increment pointer, jumping, Input/Output_
    * Includes Bitwise and Arithmatic Operators in bytes (add/subtract, multiply/divide, and/or/xor/imply/not)
    * Mutiple Tapes and pointers (Like Cufrab and DoubleFuck)
* The last programming language reference https://skillsmatter.com/skillscasts/2323-bobs-last-language http://lambda-the-ultimate.org/node/4312
* The Unicode 8.0.0 standards reference ftp://unicode.org/Public/8.0.0/charts/CodeCharts.pdf