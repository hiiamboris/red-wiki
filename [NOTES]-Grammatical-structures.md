Grammatical structures (0.6.1)

* According to natives.red produce


* if: make native! [[
		"If conditional expression is TRUE, evaluate block; else return NONE"
		cond  	 [any-type!]
		then-blk [block!]

* unless: make native! [[
		"If conditional expression is not TRUE, evaluate block; else return NONE"
		cond  	 [any-type!]
		then-blk [block!]

* either: make native! [[
		"If conditional expression is true, eval true-block; else eval false-blk"
		cond  	  [any-type!]
		true-blk  [block!]
		false-blk [block!]
	
* any: make native! [[
		"Evaluates, returning at the first that is true"
		conds [block!]

* all: make native! [[
		"Evaluates, returning at the first that is not true"
		conds [block!]

* while: make native! [[
		"Evaluates body as long as condition block returns TRUE"
		cond [block!]	"Condition block to evaluate on each iteration"
		body [block!]	"Block to evaluate on each iteration"
	
* until: make native! [[
		"Evaluates body until it is TRUE"
		body [block!]

* loop: make native! [[
		"Evaluates body a number of times"
		count [integer!]
		body  [block!]

* repeat: make native! [[
		"Evaluates body a number of times, tracking iteration count"
		'word [word!]    "Iteration counter; not local to loop"
		value [integer!] "Number of times to evaluate body"
		body  [block!]

* forever: make native! [[
		"Evaluates body repeatedly forever"
		body   [block!]

* foreach: make native! [[
		"Evaluates body for each value in a series"
		'word  [word! block!]   "Word, or words, to set on each iteration"
		series [series!]
		body   [block!]

* forall: make native! [[
		"Evaluates body for all values in a series"
		'word [word!]   "Word referring to series to iterate over"
		body  [block!]

* remove-each: make native! [[
		"Removes values for each block that returns true"
		'word [word! block!] "Word or block of words to set each time"
		data [series!] "The series to traverse (modified)"
		body [block!] "Block to evaluate (return TRUE to remove)"

* func: make native! [[
		"Defines a function with a given spec and body"
		spec [block!]
		body [block!]

* function: make native! [[
		"Defines a function, making all words found in body local"
		spec [block!]
		body [block!]
		/extern	"Exclude words that follow this refinement"

* does: make native! [[
		"Defines a function with no arguments or local variables"
		body [block!]

* has: make native! [[
		"Defines a function with local variables, but no arguments"
		vars [block!]
		body [block!]

* switch: make native! [[
		"Evaluates the first block following the value found in cases"
		value [any-type!] "The value to match"
		cases [block!]
		/default "Specify a default block, if value is not found in cases"
			case [block!] "Default block to evaluate"

* case: make native! [[
		"Evaluates the block following the first true condition"
		cases [block!] "Block of condition-block pairs"
		/all "Test all conditions, evaluating the block following each true condition"

* do: make native! [[
		"Evaluates a value, returning the last evaluation result"
		value [any-type!]
		/args "If value is a script, this will set its system/script/args"
			arg "Args passed to a script (normally a string)"
		/next "Do next expression only, return it, update block word"
			position [word!] "Word updated with new block position"

* reduce: make native! [[
		"Returns a copy of a block, evaluating all expressions"
		value [any-type!]
		/into "Put results in out block, instead of creating a new block"
			out [any-block!] "Target block for results, when /into is used"

* compose: make native! [[
		"Returns a copy of a block, evaluating only parens"
		value
		/deep "Compose nested blocks"
		/only "Compose nested blocks as blocks containing their values"
		/into "Put results in out block, instead of creating a new block"
			out [any-block!] "Target block for results, when /into is used"

* get: make native! [[
		"Returns the value a word refers to"
		word	[word! path!]
		/any  "If word has no value, return UNSET rather than causing an error"
		/case "Use case-sensitive comparison (path only)"
		return: [any-type!]

* set: make native! [[
		"Sets the value(s) one or more words refer to"
		word	[any-word! block! object! path! map!] "Word, object, map or block of words to set"
		value	[any-type!] "Value or block of values to assign to words"
		/any  "Allow UNSET as a value rather than causing an error"
		/case "Use case-sensitive comparison (path only)"
		/only "Block or object value argument is set as a single value"
		/some "None values in a block or object value argument, are not set"
		return: [any-type!]

* print: make native! [[
		"Outputs a value followed by a newline"
		value	[any-type!]

* prin: make native! [[
		"Outputs a value"
		value	[any-type!]

* equal?: make native! [[
		"Returns TRUE if two values are equal"
		value1 [any-type!]
		value2 [any-type!]

* not-equal?: make native! [[
		"Returns TRUE if two values are not equal"
		value1 [any-type!]
		value2 [any-type!]

* strict-equal?: make native! [[
		"Returns TRUE if two values are equal, and also the same datatype"
		value1 [any-type!]
		value2 [any-type!]

* lesser?: make native! [[
		"Returns TRUE if the first value is less than the second"
		value1 [any-type!]
		value2 [any-type!]

* greater?: make native! [[
		"Returns TRUE if the first value is greater than the second"
		value1 [any-type!]
		value2 [any-type!]

* lesser-or-equal?: make native! [[
		"Returns TRUE if the first value is less than or equal to the second"
		value1 [any-type!]
		value2 [any-type!]

* greater-or-equal?: make native! [[
		"Returns TRUE if the first value is greater than or equal to the second"
		value1 [any-type!]
		value2 [any-type!]

* same?: make native! [[
		"Returns TRUE if two values have the same identity"
		value1 [any-type!]
		value2 [any-type!]

* not: make native! [[
		"Returns the boolean complement of a value"
		value [any-type!]

* type?: make native! [[
		"Returns the datatype of a value"
		value [any-type!]
		/word "Return a word value, rather than a datatype value"

* stats: make native! [[
		"Returns interpreter statistics"
		/show "TBD:"
		/info "Output formatted results"
		return: [integer! block!]

* bind: make native! [[
		word 	[block! any-word!]
		context [any-word! any-object! function!]
		/copy
		return: [block! any-word!]

* in: make native! [[
		object [any-object!]
		word   [any-word! block! paren!]

* parse: make native! [[
		input [series!]
		rules [block!]
		/case
		;/strict
		/part
			length [number! series!]
		/trace
			callback [function! [
				event	[word!]
				match?	[logic!]
				rule	[block!]
				input	[series!]
				stack	[block!]
				return: [logic!]
			]]
		return: [logic! block!]

* union: make native! [[
		"Returns the union of two data sets"
		set1 [block! hash! string! bitset! typeset!]
		set2 [block! hash! string! bitset! typeset!]
		/case "Use case-sensitive comparison"
		/skip "Treat the series as fixed size records"
			size [integer!]
		return: [block! hash! string! bitset! typeset!]

* unique: make native! [[
		"Returns the data set with duplicates removed"
		set [block! hash! string!]
		/case "Use case-sensitive comparison"
		/skip "Treat the series as fixed size records"
			size [integer!]
		return: [block! hash! string!]

* intersect: make native! [[
		"Returns the intersection of two data sets"
		set1 [block! hash! string! bitset! typeset!]
		set2 [block! hash! string! bitset! typeset!]
		/case "Use case-sensitive comparison"
		/skip "Treat the series as fixed size records"
			size [integer!]
		return: [block! hash! string! bitset! typeset!]

* difference: make native! [[
		"Returns the special difference of two data sets"
		set1 [block! hash! string! bitset! typeset!]
		set2 [block! hash! string! bitset! typeset!]
		/case "Use case-sensitive comparison"
		/skip "Treat the series as fixed size records"
			size [integer!]
		return: [block! hash! string! bitset! typeset!]

* exclude: make native! [[
		"Returns the first data set less the second data set"
		set1 [block! hash! string! bitset! typeset!]
		set2 [block! hash! string! bitset! typeset!]
		/case "Use case-sensitive comparison"
		/skip "Treat the series as fixed size records"
			size [integer!]
		return: [block! hash! string! bitset! typeset!]

* complement?: make native! [[
		"Returns TRUE if the bitset is complemented"
		bits [bitset!]

* dehex: make native! [[
		"Converts URL-style hex encoded (%xx) strings"
		value [string! file!]							;@@ replace with any-string!

* negative?: make native! [[
		"Returns TRUE if the number is negative"
		number [number!]

* positive?: make native! [[
		"Returns TRUE if the number is positive"
		number [number!]

* max: make native! [[
		"Returns the greater of the two values"
		value1 [number! series! char!]
		value2 [number! series! char!]

* min: make native! [[
		"Returns the lesser of the two values"
		value1 [number! series! char!]
		value2 [number! series! char!]

* shift: make native! [[
		"Perform a bit shift operation. Right shift (decreasing) by default"
		data	[integer!]
		bits	[integer!]
		/left	 "Shift bits to the left (increasing)"
		/logical "Use logical shift (unsigned, fill with zero)"
		return: [integer!]

* to-hex: make native! [[
		"Converts numeric value to a hex issue! datatype (with leading # and 0's)"
		value	[integer!]
		/size "Specify number of hex digits in result"
			length [integer!]
		return: [issue!]

* sine: make native! [[
		"Returns the trigonometric sine"
		angle	[number!]
		/radians "Angle is specified in radians"
		return: [float!]

* cosine: make native! [[
		"Returns the trigonometric cosine"
		angle	[number!]
		/radians "Angle is specified in radians"
		return: [float!]

* tangent: make native! [[
		"Returns the trigonometric tangent"
		angle	[number!]
		/radians "Angle is specified in radians"
		return: [float!]

* arcsine: make native! [[
		"Returns the trigonometric arcsine (in degrees by default)"
		angle	[number!]
		/radians "Angle is specified in radians"
		return: [float!]

* arccosine: make native! [[
		"Returns the trigonometric arccosine (in degrees by default)"
		angle	[number!]
		/radians "Angle is specified in radians"
		return: [float!]


* arctangent: make native! [[
		"Returns the trigonometric arctangent (in degrees by default)"
		angle	[number!]
		/radians "Angle is specified in radians"
		return: [float!]

* arctangent2: make native! [[
		"Returns the angle of the point y/x in radians, when measured counterclockwise from a circle's x axis (where 0x0 represents the center of the circle). The return value is between -pi and +pi."
		y       [number!]
		x       [number!]
		return: [float!]

* NaN?: make native! [[
		"Returns TRUE if the number is Not-a-Number"
		value	[number!]
		return: [logic!]


* log-2: make native! [[
		"Return the base-2 logarithm"
		value	[number!]
		return: [float!]


* log-10: make native! [[
		"Returns the base-10 logarithm"
		value	[number!]
		return: [float!]

* log-e: make native! [[
		"Returns the natural (base-E) logarithm of the given value"
		value	[number!]
		return: [float!]

* exp: make native! [[
		"Raises E (the base of natural logarithm) to the power specified"
		value	[number!]
		return: [float!]

* square-root: make native! [[
		"Returns the square root of a number"
		value	[number!]
		return: [float!]


* construct: make native! [[
		block [block!]
		/with
			object [object!]
		/only

* value?: make native! [[
		"Returns TRUE if the word has a value"
		value
		return: [logic!]

* try: make native! [[
		"Tries to DO a block and returns its value or an error"
		block	[block!]
		/all "Catch also BREAK, CONTINUE, RETURN, EXIT and THROW exceptions"

* uppercase: make native! [[
		"Converts string of characters to uppercase"
		string		[any-string! char!]
		/part "Limits to a given length or position"
			limit	[number! any-string!]
		return: 	[any-string! char!]


* lowercase: make native! [[
		"Converts string of characters to lowercase"
		string		[any-string! char!]
		/part "Limits to a given length or position"
			limit	[number! any-string!]
		return:		[any-string! char!]


* as-pair: make native! [[
		"Combine X and Y values into a pair"
		x [integer! float!]
		y [integer! float!]

* break: make native! [[
		"Breaks out of a loop, while, until, repeat, foreach, etc"
		/return "Forces the loop function to return a value"
			value [any-type!]


* continue: make native! [[
		"Throws control back to top of loop"

* exit: make native! [[
		"Exits a function, returning no value"

* return: make native! [[
		"Returns a value from a function"
		value [any-type!]


* throw: make native! [[
		"Throws control back to a previous catch"
		value [any-type!] "Value returned from catch"
		/name "Throws to a named catch"
			word [word!]

* catch: make native! [[
		"Catches a throw from a block and returns its value"
		block [block!] "Block to evaluate"
		/name "Catches a named throw"
			word [word! block!] "One or more names"

* extend: make native! [[
		"Extend an object or map value with list of key and value pairs"
		obj  [object! map!]
		spec [block! hash! map!]
		/case "Use case-sensitive comparison"


* debase: make native! [[
		"Decodes binary-coded string (BASE-64 default) to binary value"
		value [string!] "The string to decode"
		/base "Binary base to use"
			base-value [integer!] "The base to convert from: 64, 16, or 2"

* enbase: make native! [[
		"Encodes a string into a binary-coded string (BASE-64 default)"
		value [binary! string!] "If string, will be UTF8 encoded"
		/base "Binary base to use"
			base-value [integer!] "The base to convert from: 64, 16, or 2"

* to-local-file: make native! [[
		"Converts a Red file path to the local system file path"
		path  [file! string!]
		/full "Prepends current dir for full path (for relative paths only)"
		return: [string!]


* request-file: make native! [[
		"Asks user to select a file and returns full file path (or block of paths)"
		/title	"Window title"
			text [string!]
		/file	"Default file name or directory"
			name [string! file!]
		/filter	"Block of filters (filter-name filter)"
			list [block!]
		/save	"File save mode"
		/multi	"Allows multiple file selection, returned as a block"


* request-dir: make native! [[
		"Asks user to select a directory and returns full directory path (or block of paths)"
		/title	"Window title"
			text [string!]
		/dir	"Set starting directory"
			name [string! file!]
		/filter	"TBD: Block of filters (filter-name filter)"
			list [block!]
		/keep	"Keep previous directory path"
		/multi	"TBD: Allows multiple file selection, returned as a block"

* wait: make native! [[
		"Waits for a duration in seconds"
		value [number! block! none!]
		/all "Returns all in a block"
		;/only "Only check for ports given in the block to this function"


* checksum: make native! [[
		"Computes a checksum, CRC, hash, or HMAC"
		data 	[binary! string! file!]
		method	[word!]	"MD5 SHA1 SHA256 SHA384 SHA512 CRC32 TCP hash"
		/with	"Extra value for HMAC key or hash table size; not compatible with TCP/CRC32 methods"
			spec [any-string! binary! integer!] "String or binary for MD5/SHA* HMAC key, integer for hash table size"
		return: [integer! binary!]


* unset: make native! [[
		"Unsets the value of a word in its current context"
		word [word! block!]  "Word or block of words"


* new-line: make native! [[
		"Sets or clears the new-line marker within a block or paren"
		position [block! paren!] "Position to change marker (modified)"
		value					 "Set TRUE for newline"
		/all					 "Set/clear marker to end of series"
		/skip					 "Set/clear marker periodically to the end of the series"
		size 	 [integer!]
		return:  [block! paren!]


* new-line?: make native! [[
		"Returns the state of the new-line marker within a block or paren"
		position [block! paren!] "Position to change marker"
		return:  [block! paren!]


* context?: make native! [[
		"Returns the context in which a word is bound"
		word	[any-word!]		"Word to check"
		return: [object! function! none!]

* set-env: make native! [[
		"Sets the value of an operating system environment variable (for current process)"
		var   [any-string! any-word!] "Variable to set"
		value [string! none!] "Value to set, or NONE to unset it"

* get-env: make native! [[
		"Returns the value of an OS environment variable (for current process)"
		var		[any-string! any-word!] "Variable to get"
		return: [string! none!]


* list-env: make native! [[
		"Returns a map of OS environment variables (for current process)"
		return: [map!]

* now: make native! [[
		"Returns date and time"
		/year		"Returns year only"
		/month		"Returns month only"
		/day		"Returns day of the month only"
		/time		"Returns time only"
		/zone		"Returns time zone offset from UCT (GMT) only"
		/date		"Returns date only"
		/weekday	"Returns day of the week as integer (Monday is day 1)"
		/yearday	"Returns day of the year (Julian)"
		/precise	"High precision time"
		/utc		"Universal time (no zone)"
		return: [time!]					;@@ add date! when we have it


* sign?: make native! [[
		"Returns sign of N as 1, 0, or -1 (to use as a multiplier)."
		number [number! time!]
