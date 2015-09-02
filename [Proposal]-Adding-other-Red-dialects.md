* [Dialects of different programming paradigms]
* Red/Script that acts like Python-Django, Ruby-Rails, PHP and Perl
    * A programming dialect that has the syntax structure of Python or Ruby
    * Having a Web-App library/framework that is similar to Rails or Django
* Red/Lisp that acts like Clojure, Haskell, Erlang and Scala
    * Please specify how exactly you think Clojure, Haskell, Erlang and Scala "act" / what they or their approaches have in common and how would you extract it into dialect ?
* [dialects of different internet languages]
* Having a language that works with or replaces HTML, CSS and XML families (see W3C Recommendations)
    * Dialects for HTML ands CSS should be relatively simple . But are they necessary in (especially pre 1.0) core ? (in the future, I hope)
* Having a language that works with or replaces JS, SQL, NoSQL-DB and other languages (see ISO standards) 
    * Red already is a nice dialect for Replacing JS (if someday JS gets added as compilation target).
    * Dialect for SQL generation seems _TRIVIAL_ (write it as learning experience; doesn't IMO belong in core, rather into some SQL DB interfacing lib).
    * But what exactly is "NoSQL dialect" supposed to do (http://nosql-database.org/)
    * And what should a "dialect for any other language language" accomplish ?
        * Either Red is comprehensive enough to replace other languages, or is compatible enough to use with other languages.
* Library Integration (mostly JS)
    *https://en.wikipedia.org/wiki/Solution_stack
    *https://en.wikipedia.org/wiki/List_of_Apache–MySQL–PHP_packages
* [Dialects for Easter Eggs (small bits for giggles)]
* Allow a golfing-specific script Red/Golf as an Easter Egg (similar to Sclipting, Golfscript/Flogscript, CJam, APL/J/K, Pyth, Microscript, Owk, Retina, Fourier...)
    * http://rebmu.hostilefork.com/ has done an example, but the Easter Egg would make it more powerful.
* Allow a brainfuck-like script Red/Brain as an Easter Egg
    * Classical 8-symbol BrainFuck commands (shift pointer, in/de-crement pointer, jumping, In/Out-put)
    * Includes Bitwise and Arithmatic Operators in bytes (add/subtract, multiply/divide, and/or/xor/imply/not)
    * Cutting, Copying, Pasting and Clearing data (like Emo and SPL)
    * Mutiple Tapes (like Nandypants and omnifuck) and  Multiple pointers (like Emo and Hargfak)
        * Cufrab and DoubleFuck contains both concepts

* The last programming language reference (for an idea for what Red could shoot for): https://skillsmatter.com/skillscasts/2323-bobs-last-language http://lambda-the-ultimate.org/node/4312
* The Unicode 8.0.0 standards reference: ftp://unicode.org/Public/8.0.0/charts/CodeCharts.pdf