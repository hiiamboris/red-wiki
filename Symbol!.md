Author: Peter W A Wood

Being able to declare and use unique symbols would improve program readability over using defined constants. As word! is not a valid type in Red/System, lit-word syntax could be used for symbol literals. This feature would allow the following code: 

```
#define okay 	0
#define error1	1
#define error2	2
#define error3	3

my-func: [
	i		[integer!]
 	return:		[integer!]
][
	if i > 0 [return okay]
	if i = 0 [return error1]
	if i > -100 [return error2]
	return error3
]

response: my-func my-int
switch response [
	okay 		[print "okay"]
	error1 		[print "error1"]
	error2 		[print "error2"]
	error3 		[print "error3"]
][
	
```
to be re-written as:
```
my-func: [
	i		[integer!]
 	return:		[symbol!]
][
	if i > 0 [return 'okay]
	if i = 0 [return 'error1]
	if i > -100 [return 'error2]
	return 'error3
]

print my-func my-int					;; assuming print omits the '
```