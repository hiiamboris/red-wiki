Unsorted
--------
Since a Red documentation is not yet available, I ran 'probe system/words', which prints any word known to the interpreter, in the hope of figuring out what everything does.  
This section will contain the still unsorted words while the others will contain the types and functions with basic explanation and a logical order.

 spec body words exit return   /local /extern make  none true false type  reflect value field form part limit mold only all flat absolute  add value1 value2 divide multiply negate number power exponent remainder round n to scale even down half-down floor ceiling half-ceiling sub
tract  append series  length dup count at index back clear copy
 deep types kind find case any with wild skip size last reverse tail match head
 insert  next pick poke remove select offset  if cond then-blk unless either true-blk false-blk conds while until loop word forall func function does has vars switch cases default do reduce into out  compose get set  print prin  not halt  load source  header stats show info bind context   Red yes on no off tab cr newline lf escape slash sp space null crlf dot quit-return status quit   probe quote first s second third fourth fifth action? block? cha
r? datatype? file? function? get-path? get-word? integer? issue? lit-path? lit-w
ord? logic? native? none? op? paren? path? refinement? set-path? set-word? strin
g? unset? word? any-series? spec-of body-of system version platform _context get
-words OS SET_RETURN _windows _syllable _macosx _linux else replace pattern pos
len zero? Windows? read-argument args-count none-value args args-list str simple
-io read-txt item init-console ret SetConsoleTitle as c-string! string rs-head p
rint-line rl-bind-key as-integer rl-insert-wrapper input prompt buffer allocate
ReadConsole stdin null-byte free line read-line add-history count-delimiters lis
t c escaped? in-comment? do-console red-prompt mode switch-mode eval code result
 cnt mono block q script

Types
-----
**datatype!** This is the data type of a data type.  
unset! none! logic! block! string! integer! symbol! context! word! set-word! lit-word! get-word! refinement! char! native! action! op! function! path! lit-path! set-path! get-path! paren! routine! issue! file! error! typeset! binary! closure! object! port! bitset! float! number! any-object! any-type! series! any-block! any-word! url!


Functions
---------
###Iteration###
repeat foreach map-each remove-each

###Queries###
even? odd? head? index? length? tail? equal? not-equal? strict-equal? lesser? greater?
lesser-or-equal? greater-or-equal? same? 

**type?** Returns the type of a value.  
empty?

**??** Gives some information about a function.



###IO##


Operators
---------
    + - * / = <> == =? < > <= >=

Platforms
---------
Windows Syllable MacOSX Linux

