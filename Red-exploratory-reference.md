Since a Red documentation is not yet available, I ran `red>> probe system/words`, which prints any word known to the interpreter, in the hope of figuring out what everything does. The following section will contain the still unsorted words while the others will contain the types and functions with basic explanation and a logical order.

If you know how to categorize more unsorted words, please do.

Unsorted
--------
 spec body words exit return   /local /extern   none  type   value field part limit only flat value1 value2  number  n to scale even down half-down floor ceiling half-ceiling series  length dup count at index  copy deep types kind find case any with wild skip size last reverse tail match head insert  next pick poke remove select offset  if cond then-blk unless either true-blk false-blk conds while until loop word forall func function does has vars switch cases default do reduce into out  compose get set    not halt  load source  header stats show info bind context   Red yes on no off tab cr newline lf escape slash sp space null crlf dot quit-return status quit   probe quote first s second third fourth fifth  spec-of body-of system version platform _context get-words OS SET_RETURN _windows _syllable _macosx _linux else replace pattern pos len  read-argument args-count none-value args args-list str simple-io read-txt item init-console ret SetConsoleTitle as  string rs-head print-line rl-bind-key as-integer rl-insert-wrapper input prompt buffer allocate ReadConsole stdin null-byte free line read-line add-history count-delimiters list c  do-console red-prompt mode switch-mode eval code result cnt mono block q script

Types
-----
datatype! unset! none! logic! block! string! integer! symbol! context! word! set-word! lit-word! get-word! refinement! char! native! action! op! function! path! lit-path! set-path! get-path! paren! routine! issue! file! error! typeset! binary! closure! object! port! bitset! float! number! any-object! any-type! series! any-block! any-word! url! c-string!

Functions
---------
Use `red>> ?? function-name` to learn about each function.

###Unsorted###
make reflect form mold 

###IO###
print prin

###Define###

###Iteration###
repeat foreach map-each remove-each all 

###Math###
absolute add divide multiply negate power exponent remainder round subtract  

###Series manipulations###
append back clear 

###Queries###
####Unsorted####
type? any-series? Windows? escaped? in-comment? zero?
####Math and logic####
even? odd? equal? not-equal? lesser? greater? lesser-or-equal? greater-or-equal? same?
####Series####
head? index? length? tail? empty?
####Is it this type?####
block? char? datatype? action? file? function? get-path? get-word? integer? issue? lit-path? lit-word? logic? native? none? op? paren? path? refinement? set-path? set-word? string? unset? word?  

Operators
---------
    + - * / = <> == =? < > <= >=

Values
------
###Logic###
true false
###Platforms###
Windows Syllable MacOSX Linux

