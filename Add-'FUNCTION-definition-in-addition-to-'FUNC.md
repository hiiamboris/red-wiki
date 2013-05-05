Declaring local structures in Red/System v1 requires the actual structure to be "declared" twice, once in the list of local variables and then also in the body of the function. Adding a 'FUNCTION option (as in REBOL) with three block arguments - function arguments, local variables and function body - would remove this necessity. There would only need to be a single pass of the compiler as any variable not declared in the second argument would be considered a global variable.

Code example:

	a: function [
		argument1		[integer!]
		argument2		[c-string!]
	][
		local-var1		[struct! [
			item1			[integer!]
			item2			[c-string!]
		]]
		local-var2		[integer!]
	][
		global-var: declare 	struct! [
			item1			[integer!]
			item2			[c-string!]
	
		local-var2: argument1
		.
		.
		.
		.
	]