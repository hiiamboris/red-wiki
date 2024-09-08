## How Red reacts to incomplete or missing conditions, actions as compared with R2/R3 ? 

( _Red 0.6.4 for Windows built 13-May-2021/13:13:31_ )

```
any none               ;= error
any []                 ;= none
any [ [] ]             ;= []
any [ true ]           ;= true
any [1 false]          ;= 1
any [false 2]          ;= 2
any [none false]       ;= none
any [unset 'a]         ;= unset   ====> none in R3
```

```
all none               ;= error
all []                 ;= none    ====> true in R2/R3
all [ [] ]             ;= []
all [ true ]           ;= true
all [ 1 false ]        ;= none
all [ false 1 ]        ;= none
all [ 1 2 ]            ;= 2
all [ unset 'a ]       ;= unset
all [ unset 'a false ] ;= none
all [ unset 'a true ]  ;= true
```

```
if false []            ;= none
if true []             ;= unset
if [] []               ;= unset
if true [ none ]       ;= none
if true [ [] ]         ;= []
if true 1              ;= error   ====> 1 in R3
if unset 'a []         ;= unset   ====> error in R2/R3 (cond missing or no unset)
```

```
unless false []        ;= unset
unless true []         ;= none
unless [] []           ;= none
unless false 1         ;= error   ====> 1 in R3
unless unset 'a []     ;= none    ====> error in R2/R3 (cond missing or no unset)
```

```
either true 1 2        ;= error   ====> 1 in R3
either true [] 2       ;= error   ====> unset in R3
either true [] []      ;= unset
either false [] []     ;= unset
either unset 'a [] []  ;= error
```

```
switch 1 1                 ; error
switch 1 []                ;= none
switch [] []               ;= none
switch 1 [ 1 ]             ;= none
switch 1 [ 1 [] ]          ;= unset
switch 1 [ 1 [1] ]         ;= 1
switch 1 [ 2 ]             ;= none
switch 1 [ 2 [] ]          ;= none
switch 1 [ 2 [] 1 ]        ;= none
switch 1 [ 2 [] 1 [] ]     ;= unset
switch/default 1 [ 1 ] [ ] ;= none   ====> unset in R2/R3
switch/default 1 [ 2 ] [ ] ;= unset
```

```
case []                    ;= none
case [ true ]              ;= none   ====> true in R2/R3  (1)
case [ [] ]                ;= none   ====> true in R2/R3
case [ true [] ]           ;= unset  ====> true in R2/R3
case [ [] [] ]             ;= unset  ====> true in R2/R3
case [ true false ]        ;= false
case [ false ]             ;= none
case [ false true ]        ;= none
case [ false [] true ]     ;= none   ====> true in R2/R3
case [ false true [] ]     ;= none   ====> true in R2/R3
case [ false [] true [] ]  ;= unset  ====> true in R2/R3
```

### On missing action in case, see (1) above
**@greggirwin** : it makes sense to have case/switch throw an error in case of missing action, which should be a small change in case, and just a bit more involved for switch. With the current design of case, we have consistency with any/all but not with if/unless (which error in the case of a missing argument).

### Related conversation/issues
* initial discussion on (1) :  [gitter](https://rebol.tech/gitter.im/red/help/2021/#msg609d4d8d9f2c352db109513b)
* issue opened on (1) : [red/red#4899](https://github.com/red/red/issues/4899)
* other related issues [red/red#4482](https://github.com/red/red/issues/4482) which links to [red/red#4469](https://github.com/red/red/issues/4469) and a [red/REP#85](https://github.com/red/REP/issues/85)
