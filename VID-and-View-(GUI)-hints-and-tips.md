# How do I create a scrollable `area` that is not editable?

Add a filter for 'key events (in a detect event), and return 'stop from it.

```
system/view/capturing?: yes
view [area "read only" on-detect [if event/type = 'key [return 'stop]]]
```

# How can I debug a crashing view application?

For debugging View apps, compile them with `-d` instead of `-t Windows` and run them from CMD. That will redirect the prints to the standard output.

