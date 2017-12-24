# How do I create a scrollable `area` that is not editable?

Add a filter for 'key events (in a detect event), and return 'stop from it.

```
system/view/capturing?: yes
view [area "read only" on-detect [if event/type = 'key [return 'stop]]]
```

# How can I debug a crashing view application?

For debugging View apps, compile them with `-d` instead of `-t Windows` and run them from CMD. That will redirect the prints to the standard output.

# How to handle keyboard events

> How do I read keyboard presses? I want to make a simple 2D game where I move my guy around.

```
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

```
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

```
view [a: area do [a/menu: []]]
```

# How to handle `wheel` events

```
view [
    size 200x400
    base
        on-wheel [face/offset/y: face/offset/y + (5 * event/picked)]
        on-created [set-focus face]
]
```

The key part is giving focus to the face where you define the `on-wheel` handler, so that it will receive the wheel events. By default, the window receivs those events, so you can also put an handler there:

```
view/options [size 200x400 b: base][
    actors: object [
        on-wheel: func [face event][
            b/offset/y: b/offset/y + (5 * event/picked)
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
3. Adjust the appearance and performance of Windows
4. Adjust for best appearance 
