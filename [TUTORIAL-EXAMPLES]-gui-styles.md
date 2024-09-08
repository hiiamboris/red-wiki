**WORK IN PROGRESS**
It is a first version. I gathered what I have learned and any links I got from gitter/main page.
It's made to copy-paste so If you want to add or change something make sure that there is an example.
Tested on Red 0.6.4 for Windows built 20-Jun-2020/19:24:25+02:00 commit #4d864b1
***

# Creating styles
There are 2 ways to create styles:
1. `style` keyword inside `view` block
```
view [ 
  style new-style-name: old-style rest ; declaration
  new-style-name rest ; using style
]
```
For example
```
view [
        style txt: text right on-down [print 'default-down]
        txt "Text" red
]
```
2. extending `system/view/VID/styles` 
```
extend system/view/VID/styles [
    style-name: [
            default-actor: some-actor
            template: [
                type: 'some-view-type
                size: sizeXsize ; 
                ;... ; rest of the field & user defined fields
                actors: [
                    on-actor: som-function-definition[face-argument event-argument] [ 
                        code here
                    ]
                ]  
            ]
            init: [
                this code initialize your styles
            ]
        ]
]
view [ style-name rest-of-arguments ]
```
For example:
```
extend system/view/VID/styles [
    style-name1: [
            template: [
                type: 'base
                size: 200x200
                color: red
            ]
        ]
]
view [ style-name1]
```
or more complex: 
```
extend system/view/VID/styles [
    style-name2: [
            default-actor: on-down
            template: [
                type: 'base
                size: 200x200
                color: red
                actors: [
                    on-down: function[face event] [ 
                        print 'default-on-down
                    ]
                ]  
            ]
            init: [
                face/color: random 255.255.255
            ]
        ]
]

view [ style-name2]
```

# Triggering an actor
We can trigger an actor using `do-actor` function:
```
view [
    style b: base
        on-down [ do-actor face event 'change ]
        on-change [print 'changed]
    b "click me"
]
```
`on-actor` are normal functions so we can call it like `face/on-actor face event` instead of `do-actor...`. `do-actor` has some error checking (like checking whenever or not the event is supported) but it is intended for internal use. It can be changed or removed.

# Default actor
Default actor is triggered without `on-actor` text, as in `view [base [print 'default-actor] ]`. Here, default actor is `on-down` (mouse press) and it runs `[print 'default-actor]`.   
We can specify a default actor like this:
```
extend system/view/VID/styles [
	default-style:		[
        default-actor: on-down
        template: [
            type: 'text  size: 100x25 color: red           
        ]
    ]
]
view [default-style [print 'default-actor]]
```
If we do not specify an actor (no `default-actor: ...` line) and we try to run our code we will get an error (may change in the future version of the Red):
```
view [default-style [print 'default-actor]]
; *** Script Error: make-actor does not allow none for its <anon> argument
; *** Where: make-actor
; *** Stack: view layout 
```
# Custom fields

We can add custom fields (values, function etc) with `extra` or in the `template`. Extra
```
view [
    style base': base red extra [
        a: 1
        b: 2
    ]
    b1: base' 100x100 
    b2: base' 100x100
    do [
        b1/extra/a: 42 ; we just change one extra/a of b1
    ]
]
print b1/extra ; 42 2
print b2/extra ; 1 2
```
Template:
```
extend system/view/VID/styles [
	text':		[
        template: [
            type: 'text  
            size: 100x25 
            var-a: 0
            var-b: 1
        ]
    ]
]

view [
    t1: text' "foo" with [var-a: 21 var-b: 42] 
    t2: text' "foo"  
]

print ['t1 'var-a t1/var-a 'var-b t1/var-b]
; t1 var-a 21 var-b 42
print ['t2 'var-a t2/var-a 'var-b t2/var-b]
; t2 var-a 0 var-b 1
```

## Global custom fields

Setting fields with `with` will not set a field for each style. The field will be kept in one place:
```
view [
    style text'': text
        100x25
        with [
            var-a: 1
            var-b: 2
            
            actors: [
                on-down: function [face event] [
                    print 'on-down
                    print [face/text var-a var-b]
                ]
            ]
        ]   
    t2: text'' "first"
    t3: text'' "second" with [var-a: 11 var-b: 42]
]
```
will output the same values:
```
on-down
first 11 42
on-down
second 11 42
```

We can use it for counting how many styles are rendered. Here I just change face/text to n-th number:
```
view [
    style text': text red
        100x25
        with [
            count: 0
            actors: [
                on-down: function [face event] [
                    print 'on-down
                    print ['count count]
                ]
                on-create: function [face event /extern count]  [
                    count: (1 + count)
                    case [
                        count = 1 [
                            face/text: "1st"
                        ]
                        count = 2 [
                            face/text: "2nd"
                        ]
                        count = 3 [
                            face/text: "3rd"
                        ]
                        true [
                            face/text: rejoin [count "th"]
                        ]
                    ]
                ]
            ]
        ]

    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return 
]
```
Or we can share some values between styles. Here I am using 2 words holding position in 2D space (multiplication table). Every style's text changes, sequentially when we run `on-create` function. :
```
view [
    style text': text red
        25x25
        with [
            x-axis: 1
            y-axis: 1
            actors: [
                on-create: function [face event /extern x-axis y-axis]  [
                    face/text: to-string (x-axis * y-axis)
                    x-axis: x-axis + 1
                    if x-axis > 10 [
                        x-axis: 1
                        y-axis: y-axis + 1
                    ]
                ]
            ]
        ]

    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return
    text' text' text' text' text' text' text' text' text' text' return 
]
```
One thing we should be aware is that `on-create` runs after we go through `view` block. For example if we use many `with` it will run each `with` before runnning any `on-create` function. Every `with` will change a word so the value will be from the last `with`. Here, all `text'` styles have `y-axis` set to 2 (only 2nd row should be set to 2):
```
view [
    style text': text red
        100x25
        with [
            x-axis: 0
            y-axis: 0
            actors: [
                on-create: function [face event /extern x-axis y-axis]  [
                    face/text: to-string (x-axis * y-axis)
                    x-axis: x-axis + 1

                ]
            ]
        
        ]

    text' with[y-axis: 1 x-axis: 1] text' text' text' text' text' text' text' text' text' return    
    text' with[y-axis: 2 x-axis: 1] text' text' text' text' text' text' text' text' text' return    
]
```

# Initialization
`init [<body>]` styling option allows you to define a block of code to run when the style is instantiated. The difference is that `init` should be used internally by the style, while `on-create` should be for the user to override. You can still use both.

```
extend system/view/VID/styles [
    now-text: [
        default-actor: on-down 
        template: [
            type: 'base
            size: 100x100
            actors: [
                on-create: function [face event] [
                    face/color: red
                ]
            ]
        ]
        init [
            print 'initiation
            face/data: now/time
            print ["Set text to: " now/time]
        ]
    ]
]
view [now-text]
```
```
view [
    style b: base 
        on-create [
            ;face/color: red
        ]
    init [face/color: red]
    b "foo"
]
```

### Run order
```
extend system/view/VID/styles [
    now-text: [
        default-actor: on-down 
        template: [
            type: 'base
            size: 100x100
            actors: [
                on-create: function [face event] [
                    print 'on-create
                    print now/time
                ]
                on-created: function [face event] [
                    print 'on-created
                    print now/time
                ]
            ]
        ]
        init [
            print 'initiation
            print now/time
            wait 2
        ]
    ]
]
view [now-text]
; initiation
; 17:32:14
; on-create
; 17:32:16
; on-created
; 17:32:16
```

See https://github.com/red/REP/issues/82 for the **dangers of using `init`.**

## Difference between `init` & `on-create`
Only (?) with `init` you can change a face:
```
extend system/view/VID/styles  [
    now-text: [
        default-actor: on-down 
        template: [
            type: 'text
            size: 100x100
        ]
        init [
            print 'initiation
            face: make-face 'base
        ]
    ]
] 
view [n: now-text red  ] print n/type
; initiation
; base
```

# Custom actors/events

1. Add `on-custom-actor` like with other actors.
2. Add actor pair (actor on-actor) in the `system/view/evt-names` 

Click the face 4 times to trigger a custom actor:
```
extend system/view/VID/styles [
	text':		[
        template: [
            type: 'text  
            size: 100x25
            color: red
            var: 0
            actors: [
                on-down: function [face event] [
                    print 'on-down 
                    if face/var > 2 [
                        do-actor face event 'custom-actor
                        face/var: 0
                    ]
                    face/var: face/var + 1
                    
                ]
                on-custom-actor: function [face event] [print 'custom-actor]
            ]
        ]
    ]
]
unless system/view/evt-names/custom-actor [append system/view/evt-names [custom-actor on-custom-actor]]

view [text' "foo"]
```
If you do not add to evt-names this will show: 
```
; *** Script Error: in does not allow none for its word argument
; *** Where: in
; *** Stack: view do-events do-actor do-safe do-actor 
```
# More about `init` & `with`
They both can be used for setting fields but they are more powerful. They bind (link to) the face object and they evaluate the block. We can run any code here.
```
view [
    style foo: base "basic text" red 
        with [
            probe text: to-string data: random 42
        ]
    foo blue
]
```
Here I am using `probe`, `to-string` and `random` functions.

# Source, links etc
https://rebol.tech/gitter.im/red/help/2020/#msg5f033e92fa0c9221fc816712  
https://www.red-lang.org/2017/07/063-macos-gui-backend.html  
https://www.red-lang.org/2019/02/january-2019-update.html  
https://rebol.tech/gitter.im/red/help/2020/#msg5ef826d0fa0c9221fc6543af  
https://rebol.tech/gitter.im/red/red/gui-branch/2020/#msg5ef9beae47fdfd21edf08cc0  
https://rebol.tech/gitter.im/red/help/2020/#msg5f035196405be935cde4f71d
