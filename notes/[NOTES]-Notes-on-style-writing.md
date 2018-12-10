Life of custom style author can certainly be made easier. Here are some thoughts how:

## Default actor

Currently, adding action to a custom style in VID overwrites custom actors:

```red
view [
    style my-button: base 100x24 on-create [
        face/draw: compose [pen black fill-pen white box 0x0 100x24 text 2x2 (face/text)]
    ]
    m1: my-button "hello"
    m2: my-button "world" [print "test"]
]
```

Such action (`print "test"`) should be added to some default location instead (like `on-action` for example) and it should be up to style author, when it will be called. It's fine to call it in `on-click` for some style, but different ones may prefer `on-over` (for dragging) or other actors.

## Passing values from VID to style

It's easy for basic values like text, color, etc that are supported by VID already, but setting some custom values is not possible. Using `extra` will overwrite whole `extra` context, so I think there should be a way to just change specific words in `extra` context.

## Example

An explanation & example style can be found: [Writing simple VID style](http://red.qyz.cz/writing-style.html)