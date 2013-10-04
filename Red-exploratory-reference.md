Since a Red documentation is not yet available, I ran `red>> probe system/words`, which prints any word known to the interpreter, in the hope of figuring out what everything does. The following section will contain the still unsorted words while the others will contain the types and functions with basic explanation and a logical order.

If you know how to categorize more unsorted words, please do.

Unsorted
--------
 body exit return   /local /extern  none  count at   offset   map-each remove-each conds          halt  load source  header stats show info bind context   Red yes on no off tab cr newline lf escape slash sp space null crlf dot quit-return status quit   probe quote s second third fourth fifth  spec-of body-of system version platform _context get-words OS SET_RETURN _windows _syllable _macosx _linux else replace pattern pos len  read-argument args-count none-value args args-list str simple-io read-txt item init-console ret SetConsoleTitle as  string rs-head print-line rl-bind-key as-integer rl-insert-wrapper input prompt buffer allocate ReadConsole stdin null-byte free line read-line add-history count-delimiters list c  do-console red-prompt mode switch-mode eval code result cnt mono block q script

###Refinements or arguments - Don't sort these###
value field types part limit only flat value1 value2 number n to scale even down half-down floor ceiling half-ceiling deep kind series word words length case with wild size reverse match dup index spec type cond then-blk true-blk false-blk vars cases default into out

Types
-----
datatype! unset! none! logic! block! string! integer! symbol! context! word! set-word! lit-word! get-word! refinement! char! native! action! op! function! path! lit-path! set-path! get-path! paren! routine! issue! file! error! typeset! binary! closure! object! port! bitset! float! number! any-object! any-type! series! any-block! any-word! url! c-string!

Operators
---------
    + - * / = <> == =? < > <= >=

Values
------
###Logic###
true false
###Platforms###
Windows Syllable MacOSX Linux

Functions
---------
Use `red>> ?? function-name` to learn about each function.

###Unsorted###
make reflect form mold copy reduce compose get set

###IO###
print prin

###Define###
func function does has

###Flow control###
if unless either repeat foreach while until loop forall all any switch do 

###Math and logic###
absolute add divide multiply negate power exponent remainder round subtract 

###Series###
####Queries####
head? index? length? tail? empty? skip find head tail first last back next pick select 
####Manipulations####
append clear insert poke remove 

###General queries###
####Unsorted####
type? any-series? Windows? escaped? in-comment? zero?
####Math and logic####
even? odd? equal? not-equal? lesser? greater? lesser-or-equal? greater-or-equal? same?
####Is it this type?####
block? char? datatype? action? file? function? get-path? get-word? integer? issue? lit-path? lit-word? logic? native? none? op? paren? path? refinement? set-path? set-word? string? unset? word?  
