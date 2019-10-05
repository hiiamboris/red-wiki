Current (pre fast-lexer) results for `LOAD` with invalid inputs:

```
Value      Result
"4x"       [invalid pair!]
"4a"       [invalid integer!]
".4a"      [invalid float!]
"1.0x.0"   [invalid pair!]
".4x"      [0.4 x]
".0x.0"    [0.0 x.0]
```

Here is the test script (feel free to extend the inputs and update the output above)
```Red
print "Value      Result"
foreach value [
	"4x"
	"4a"
	".4a"
	"1.0x.0"
	".4x"
	".0x.0"
] [
	prin [mold value pad copy "" 8 - length? value]
	probe either error? result: try [load value] [reduce [result/id result/arg1]] [result]
]
```