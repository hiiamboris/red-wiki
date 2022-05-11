# How do I create a scrollable `area` that is not editable?

Add a filter for 'key events (in a detect event), and return 'stop from it.

```red
system/view/capturing?: yes
view [area "read only" on-detect [if event/type = 'key [return 'stop]]]
```

# How can I debug a crashing view application?

For debugging View apps, compile them with `-d` instead of `-t Windows` and run them from CMD. That will redirect the prints to the standard output.

# How to handle keyboard events

> How do I read keyboard presses? I want to make a simple 2D game where I move my guy around.

```red
view [
    size 300x300
    base red on-created [set-focus face] on-key [
        switch event/key [
            up    [face/offset/y: face/offset/y - 5]
            down  [face/offset/y: face/offset/y + 5]
            left  [face/offset/x: face/offset/x - 5]
            right [face/offset/x: face/offset/x + 5]
        ]
    ]
]
```

Notice that you need to set-focus on a given face in order to receive key event there. An alternative way is to hook the event handler to the window itself:

```red
view/options [
    size 300x300
    b: base red
][
    actors: context [
        on-key: func [face event][
            switch event/key [
                up    [b/offset/y: b/offset/y - 5]
                down  [b/offset/y: b/offset/y + 5]
                left  [b/offset/x: b/offset/x - 5]
                right [b/offset/x: b/offset/x + 5]
            ]
        ]
    ]
]
```

# How can I disable the standard context menu for a face?

Set its menu facet to an empty block. For example:

```red
view [a: area do [a/menu: []]]
```

# How to handle `wheel` events

```red
view [
    size 200x400
    base
        on-wheel [face/offset/y: to integer! round face/offset/y + (5 * event/picked)]
        on-created [set-focus face]
]
```

The key part is giving focus to the face where you define the `on-wheel` handler, so that it will receive the wheel events. By default, the window receivs those events, so you can also put an handler there:

```red
view/options [size 200x400 b: base][
    actors: object [
        on-wheel: func [face event][
            b/offset/y: to integer! round b/offset/y + (5 * event/picked)
        ]
    ]
]
```

See also: https://doc.red-lang.org/en/view.html#_event_datatype

# Color transparency on Windows 7
> Why are the colors `glass` and `transparent` showing up as pitch black?

Windows 7 must be set to 'best appearance' for transparency to work:
1. Control Panel
2. System
3. Performance Information and Tools
4. Adjust visual effects
5. Adjust for best appearance

# How to move a face to the top of the z-order

```red
move-to-top: func [face] [move find face/parent/pane face tail face/parent/pane]
```

# Styles

## Styles are a single face

```red
view [
    style buttons: panel green [
        below
        button "hello"
        button "goodbye"
    ] 

    buttons
    buttons
]
```
The above won't work because a style is just one face. A panel can have a tree of child faces, so can't be used as a style. Styles are there to customize the look of widgets, not to instantiate layouts made of several faces. If one needs to duplicate VID code beyond what styles can achieve, use regular Red constructions, like `compose` to build VID blocks dynamically.

# How to set up default custom font?

1. Change system template, e.g.:
```
append system/view/VID/styles/text/template [
    font: make font! [
        color: red size: 20 name: "Courier New"
    ]
]
```
This method affects only styles for which you change template, of course. To change templates for several styles you can use the following function:
```
set-defaults-for-styles: function [styles [block!] defaults [block!]][
    foreach style styles [
        append system/view/VID/styles/:style/template defaults
    ]
]
```
With this method you can use several facets for several styles at once.

2. The other way is to change `system/view/VID/default-font`, e.g.:
```
system/view/VID/default-font: [
    name fname size fsize color fcolor
]
fname: "Courier New" fsize: 16 fcolor: orange 
```
You cant use set-words and values directly because of the internal way `default-font`is [used](https://github.com/red/red/blob/master/modules/view/VID.red#L432).

With this method you still need to "touch" `font` facet of the styles, in which you want the custom font to be displayed, e.g. in this manner:
```
view [below 
    text "Second way" font [] 
    button font [] "On all styles" 
    tab-panel 200x50 font [] ["A" [] "B" []]
]
```
But you can also change any aspect of your custom font, e.g.:
```
view [text "Hello" font-size: 20]
```
Custom font is invoked whenever you mention font facet in your VID block.

# How to pass data to second window

One possibility is to open second window in a function and pass data as argument(s) to this function, e.g.:
```
context [
    obj1: ar1: lay: none
    win: func [obj /local obj2 ar2][
        view/options [
            ar2: area with [text: mold body-of obj] 
            button "Back" [obj2: object load ar2/text unview] 
        ][offset: lay/offset + as-pair lay/size/x -25]
        obj2
    ]
    view lay: layout [
        ar1: area "[a: 1 b: 2]"
        button "Window2" [
            obj1: win object load ar1/text 
            ar1/text: mold body-of obj1
        ]
    ]
]
```

The other possibility is just to open the window and set data on its faces directly, e.g.:
```
view [below 
    tabs: field "one two" 
    panels: area {[text "1"]^/[button "OK" [unview]]} 
    button "Tabs-win" [
        view [tab-panel 200x100 with [
            data: split tabs/text space 
            pane: copy [] foreach panel load panels/text [
                append pane layout/only compose/only [panel (panel)]
            ]
        ]]
    ]
]
```

# How to create a number-only field

Two examples, one - more articulated - with `parse`, the other - more contracted - with `find`:

```
context [
    system/view/capturing?: yes
    digit: charset ["0123456789.-^(back)^C^V"]
    special: ['end | 'home | 'left | 'right | 'delete]
	
    numeric-key?: function [event /local ok?] [
        if event/type = 'key [
            ok?: either (char? event/key) [
                parse to-string event/key [some digit]
            ][
                parse to-block event/key special
            ]
            if not ok? [return 'stop]
        ]
    ]
	
    view [
        title "Test"
        text "Numeric field:"
        num: field 150 right focus on-detect [numeric-key? event]
    ]
]
```

```
context [
    system/view/capturing?: yes
    digit: charset "0123456789.'-^(back)^C^V"
    
    numeric-key?: func [event] [
        all [
            event/type = 'key 
            not find either char? event/key [digit][
                [end home left right delete]
            ] event/key
            'stop
        ]
    ]

    view [
        title "Test"
        text right "Numeric field:"
        field 150 right focus on-detect [numeric-key? event]
    ]
]
```

# Panel layouts

Panels are just sub `layout`s, but can have an optional divider value for grid layouts. Something to note is that the divider value is used based on `across/below` for the panel. If you use `across`, the default, it's the number of columns, for `below` it's the number of rows. You can choose what's best for each case. As an example, you could make the `text` faces wider here, to prevent the text lists from overlapping, or you can use the second example and the `text-list` widths are taken into account.

```
panel 3 [
	text "A"  
	text "B" 
	text "C"   
	text-list data ["a" "aa" "aaa"]
	text-list data ["b" "bb" "bbb"]
	text-list data ["c" "cc" "ccc"]
]
```

```
panel 2 [
	below
	text "A"  
	text-list data ["a" "aa" "aaa"]
	text "B" 
	text-list data ["b" "bb" "bbb"]
	text "C"   
	text-list data ["c" "cc" "ccc"]
]
```

# How to create a custom title bar for a window

From @hiiamboris. `/flags 'no-title` is used to suppress the standard title bar, and `/tight` is used so faces fit right to the edge of the OS window, without any padding around them.

```
p: none
view/flags/tight [
    ; Main title bar area
    base 300x20
        on-up [p: none]
        on-down [p: event/offset]
        all-over on-over [
            if p [
                fp: face/parent fp/offset: fp/offset + event/offset - p
            ]
        ]
        draw [
            line-width 0 fill-pen linear magenta cyan box 0x0 300x20 text 1x1 "My custom title bar"
        ]
    ; Close box
    base 20x20 cyan
        draw [line 3x3 16x16 line 3x16 16x3]
        on-down [unview] return
    ; Client area
    base white 320x300 "workspace"
] 'no-title
```

# Changing text list text color

You can't make each item a different color, but you can set the color based on what's selected. Credit to @hiiamboris.

```
view [
    t: text-list white black data ["red" "green" "blue"] on-change [t/font/color: do t/data/(t/selected)]
]
```

# Base face can hold only base faces in its pane (sub-layout)

GDI+ could hold other faces too, but had some problems also; D2D can hold only base faces.

# When to use `options`, `extra` and `with`

Let's say you create a template for a new style and want to add a `config` to set  it up. Where to put it? You have at least three options: create a new facet in template, put your config in `options` facet, or put it in `extra` facet. 

Advice from @hiiamboris: If `config` is a facet that lives with the widget, `with` is the most direct way to work with it, and it will be easier to access if one wants. If it's just data that gets read and discarded then `options` wins because it doesn't require you to allocate a facet for it. Or if you think this data is not going to be accessed by anyone, thus you kind of hide it.

`extra` is perhaps best to keep free and available for users to deploy.