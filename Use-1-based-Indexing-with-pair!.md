Currently in Rebol, `pick` and `poke` seem to use 0-based indexing with `pair!`. This is confusing and inconsistent with the rest of Rebol and Red which uses 1-based indexing.

Example:

```red
img: make image! [2x2 #{FF000000FF000000FF000000} #{000000FF}]
probe pick img 1
probe pick img 2
probe pick img 3
probe pick img 4
print "---------"
probe pick img 1x1
probe pick img 2x1
probe pick img 1x2
probe pick img 2x2
print "---------"
probe pick img 0x0
probe pick img 1x0
probe pick img 0x1
probe pick img 1x1
```

Current Rebol result:

```red
255.0.0.0
0.255.0.0
0.0.255.0
0.0.0.255
---------
0.0.0.255
none
none
none
---------
255.0.0.0
0.255.0.0
0.0.255.0
0.0.0.255
```

Preferred result:

```red
255.0.0.0
0.255.0.0
0.0.255.0
0.0.0.255
---------
255.0.0.0
0.255.0.0
0.0.255.0
0.0.0.255
---------
none
none
none
255.0.0.0
```