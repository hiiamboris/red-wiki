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