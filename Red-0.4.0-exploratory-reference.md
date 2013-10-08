Since a Red documentation is not yet available, I ran `red>> probe system/words`, which prints any word known to the interpreter, in the hope of figuring out what everything does. This document categorizes the words known by the Red interpreter in order to supply a basic language reference.

Types
-----
datatype! unset! none! logic! block! string! integer! symbol! context! word! set-word! lit-word! get-word! refinement! char! native! action! op! function! path! lit-path! set-path! get-path! paren! routine! issue! file! error! typeset! binary! closure! object! port! bitset! float! number! any-object! any-type! series! any-block! any-word! url! c-string!

Operators
---------
    + - * / = <> == =? < > <= >=

Values
------
###Logic###
true false yes no on off Red
###Character constants###
tab cr newline lf escape slash sp space null crlf dot

Functions
---------
Use `red>> ?? function-name` to learn about each function.

###Unsorted###
make reflect copy reduce compose get set halt load stats bind quit-return quit probe quote spec-of body-of system read-argument init-console do-console q

###IO###
print prin input 

###Define###
func function does has

###Flow control###
if unless either repeat foreach while until loop forall all any case switch do 

###Math and logic###
absolute add divide multiply negate power exponent remainder round subtract 

###Series###
####Queries####
head? index? length? tail? empty? at skip find head tail first second third fourth fifth last back next pick select 
####Manipulations####
append clear insert poke remove replace

###General queries###
####Unsorted####
type? any-series? Windows? escaped? in-comment? zero?
####Math and logic####
even? odd? equal? not-equal? lesser? greater? lesser-or-equal? greater-or-equal? same?
####Is it this type?####
block? char? datatype? action? file? function? get-path? get-word? integer? issue? lit-path? lit-word? logic? native? none? op? paren? path? refinement? set-path? set-word? string? unset? word?  

###Misc###
####To string####
form mold

Refinements or arguments
------------------------
The following words are being printed when executing `probe system/words` but they are not globally available, mostly because they are local to built in functions.

Windows Syllable MacOSX Linux body exit return /local /extern none count offset map-each remove-each conds s else args-count none-value args args-list str simple-io read-txt item ret SetConsoleTitle as string rs-head print-line rl-bind-key as-integer rl-insert-wrapper prompt buffer allocate ReadConsole stdin null-byte free line read-line add-history count-delimiters list c   red-prompt mode switch-mode eval code result cnt mono block script value field types part limit only flat value1 value2 number n to scale even down half-down floor ceiling half-ceiling deep kind series word words length with wild size reverse match dup index spec type cond then-blk true-blk false-blk vars cases default into out source header show info context status version platform _context get-words OS SET_RETURN _windows _syllable _macosx _linux pattern pos len

