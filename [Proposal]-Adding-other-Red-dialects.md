* [Dialects of different programming paradigms]
* Red/Script that acts like Python-Django, Ruby-Rails, PHP and Perl
    * A programming dialect that has the syntax structure of Python or Ruby
        * If one wants to write Ruby, Python or whatever, he/she/it better use Ruby, Python or whatever ;-). This would provide no benefit for Red, and most people working on / waiting for Red do so precisely because they appreciate Red is not like Python or Ruby.
        * Since it is said that Red is the first "Full Stack Language", it would be good to at least interest students who learnt Ruby or Python in school to have the incentives to learn Red once they finish their curriculum ("If you can do Ruby/Python, you can do Red even better!").
    * Having a Web-App library/framework that is similar to Rails or Django
        * In the sense of capable-enough solution, it will only take a "moderately finished" Red and some time before it will start happening (more accessible to general [coder] population)
        * But it will of course look different and I'd bet it will be more simple (less noise in the templates / code / routing etc.)
* Red/Lisp that acts like Clojure, Haskell, Erlang and Scala
    * Please specify how exactly you think Clojure, Haskell, Erlang and Scala "act" / what they or their approaches have in common and how would you extract it into dialect ?
    * also, Red is lispy-enough already (dialects/PARSE/different evaluation modes for arguments)
    * Is it possible to contain all lisp-like functions from these languages?

* [dialects of different internet languages]
* Having Red/Web that works with or replaces HTML, CSS and XML families (see W3C Recommendations)
    * Dialects for HTML ands CSS should be relatively simple . But are they necessary in (especially pre 1.0) core ? (in the future, I hope)
        * As soon as first web frameworks start to appear, we can be sure someone will write them (because HTML & CSS suck ;-)
* Having Red/Database that works with or replaces JS, SQL, NoSQL-DB and other languages (see ISO standards) 
    * Red already is a nice dialect for Replacing JS (if someday JS gets added as compilation target).
    * Dialect for SQL generation seems _TRIVIAL_ (write it as learning experience; doesn't IMO belong in core, rather into some SQL DB interfacing lib).
    * But what exactly is "NoSQL dialect" supposed to do (http://nosql-database.org/)
        * But a dialect is basically a notation, a way to describe something (other code can assign it some meaning, or transform it into another form). NoSql is cover-term for a bunch od solutions for persisting data. [Hyper]Graph databases, key/value stores, document databases and [native] object store databases, there IMO cannot be "one dialect to talk to them them all". Start by bindings and dialects for querying concrete nosql stores, and where regularities will arise, you can abstract over them later.
    * And what should a "dialect for any other language language" accomplish ?
        * Either Red is comprehensive enough to replace other languages, or is compatible enough to use with other languages.
            * No dialects needed. Red, Red/System. If it is necesary to interface leggacy code, there are (or will be) C ABI compatibility, Java/.Net/Cocoa bridges, sockets & protocols, what more is needed ?
                * I assume Red/System --> Red (+ Red/Script) --> Red/Web (+ Red/Database ?)
    * Library Integration (mostly JS)
        * What about SQL?
    *https://en.wikipedia.org/wiki/Solution_stack
    *https://en.wikipedia.org/wiki/List_of_Apache–MySQL–PHP_packages

* [Dialects for Easter Eggs (relatively small bits for giggles, only release for milestones)]
    * excessive easter eggs don't IMO belong into compiler/stack that's trying to be serious(ly small ;-)
* Allow a brainfuck-like script Red/Brain as an Easter Egg (1st milestone)
    * Classical 8-symbol BrainFuck commands (shift pointer, in/de-crement pointer, jumping, In/Out-put)
    * Includes Bitwise and Arithmatic Operators in bytes (add/subtract, multiply/divide, and/or/xor/imply/not)
    * Cutting, Copying, Pasting and Clearing data (like Emo and SPL)
    * Mutiple Tapes (like Nandypants and omnifuck) and  Multiple pointers (like Emo and Hargfak)
        * Cufrab and DoubleFuck contains both concepts
* Allow a golfing-specific script Red/Golf as an Easter Egg  (2nd milestone)
    * similar to Sclipting, Golfscript/Flogscript, CJam, APL/J/K, Pyth, Microscript, Owk, Retina, Fourier...
    * http://rebmu.hostilefork.com/ has done an example, but the Easter Egg would be more powerful than Rebmu.
    * Allows for data compression into base32, base64, base85 and/or base 91 to win byte-based competitions.


* The last programming language reference (for an idea for what Red could shoot for): https://skillsmatter.com/skillscasts/2323-bobs-last-language http://lambda-the-ultimate.org/node/4312
* The Unicode 8.0.0 standards reference: ftp://unicode.org/Public/8.0.0/charts/CodeCharts.pdf