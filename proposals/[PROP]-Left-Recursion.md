```red
;example taken from red web page

expr:   [term ["+" | "-"] expr | term]
term:   [factor ["*" | "/"] term | factor]
factor: [primary "**" factor | primary]
primary:[some digit | "(" expr ")"]

;antlr4 grammar, converted to red, which has left recursion, would look something like this...

expr: [  
    expr ["*" | "/"] expr
    | expr ["+" | "-"] expr
    | some digit
    | "(" expr ")"
] 

; the actual antlr4 grammar is here for reference
expr: expr ('*'|'/') expr
    | expr ('+'|'-') expr
    | INT
    | '(' expr ')'
    ;
```