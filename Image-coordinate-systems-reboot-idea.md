# Image coordinate systems reboot

See https://github.com/red/red/issues/3504 for the history.

See https://github.com/red/REP/issues/34 for the discussion.

Currently there are 3 different coordinate systems (CS) used in images:

### 1. Pixel access CS

Spans [1x1; image/size]. Addresses individual pixels. Used in accessing image pixel colors (pick, poke, path notation):

`image/1x1`
`image/(as-pair x y): color`
etc.

### 2. View and Draw CS

Spans [-0.5x-0.5; image/size - 0.5x0.5]. Integral values refer to image pixel centers. Used in Draw and View commands:

`draw ... [line 0x0 100x0]`
`view [at 0x0 base 100x100]`
etc.

This CS is good at drawing pixel boundary aligned graphics with odd line widths (1,3,5...). It doesn't work for any even line width.

### 3. Image CS

It is the most obvious way to think of an image of size WxH: as an area between the points (0,0) and (W,H). So spans [0x0; image/size].

Some draw commands: `image` and `clip` are using this CS.

This CS is good at drawing pixel boundary aligned graphics with even line widths (0,2,4,6...). It doesn't work for any odd line width.


# Problems

### 1. Conversion from one CS to another is a place ripe for silly bugs and is a repelling operation for beginners and non mathematically obsessed people. And there are no available means for that so everyone has to write his own.

![](https://i.gyazo.com/c12430002b722355757579d6bb986149.png)

CS-1 and CS-2 share no common points at all:
`line 0x0 2x0` (CS-2) is drawn on pixels `[1x1 2x1 3x1]` (CS-1). Point 1x1 in CS-2  does not even touch the corners of the pixel 1x1 in CS-1.

Let's map one CS into another:

Coordinates of the uppermost leftmost pixel are:
a single point `1x1` in CS-1
an area between `-0.5x-0.5` and `0.5x0.5` in CS-2
an area between `0x0` and `1x1` in CS-3

To move from CS-2 into CS-3:
```
scale 2 2 translate 1x1 scale 0.5 0.5 <commands>
```

To move from CS-3 into CS-2:
```
scale 2 2 translate -1x-1 scale 0.5 0.5 <commands>
```

Conversion between CS-1 and others depends on what one's doing.

Also, worth noting that iteration over CS-1 is simply noisy. Note the amount of extra +/- operations when both *x,y* and linear index *i* are required:
```
repeat i W * H [xy: as-pair  i - 1 % W + 1  i - 1 / W + 1  ...]
```
```
repeat y H [repeat x W [i: y - 1 * W + x - 1  ...]]
```
Look also into the `inspect` function down there. In my experience I have always had to manually undo the 1-based indexing, fighting against the language. But this is personal and your experience may be different.

### 2. As pairs currently are represented by integer numbers, draw operations are forced to snap onto a grid consisting of pixel centers.

This makes it impossible to use scale/rotate/transform operations without going into tricks. Working directly with a transformation matrix is beyond normal people's knowledge.

Example. 90 degree rotation.

A simplest way to rotate an image of size *WxH* by 90 degrees is: `rotate 90 center`, *where center has to be a pixel center* or the result will be blurry. In CS-2 this only works if both *W* and *H* are odd. In CS-3 this only works if both *W* and *H* are even.

Another relatively easy way to rotate an image of size *WxH*: `translate Wx0 rotate 90 0x0`. This works only in CS-3, though for any W and H.

For CS-2 the easiest acceptable solution is: `translate (as-pair W - 1 0) rotate 90 0x0`. How obvious it is to newcomers? ☺


### 3. At some point a floating-point pair type will have to be introduced, without which it will be very tricky to work with vector graphics (due to mentioned grid mainly). Tying this new type to CS-2 will lead only to more complications:

Example. Scaling by 2.

Suppose (using the float-pair) one draws an opaque monochrome box spanning an area 10x10 pixels in the top left corner:
```
draw 10x10 [pen off  fill-pen color  box -0.5x-0.5 9.5x9.5]
```

Or currently in CS-2 it can be done as:
```
draw 10x10 [line-width 1  pen color  fill-pen color  box 0x0 9x9]
```
(9x9 - 0x0) is less than the image size by 1x1 (line-width x 1x1), and that has to be accounted for. No big deal, but when you have to do that for every shape drawn...

And suppose then one decides to scale that up twice. One can't simply multiply each coordinate by 2. Neither one can add a `scale 2 2` command. One has to resort to this:
```
draw 20x20 [pen off  fill-pen color  box -0.5x-0.5 19.5x19.5]
```

Or in current implementation:
```
draw 20x20 [line-width 1  pen color  fill-pen color  box 0x0 19x19]
```
Again, line width is to be accounted for.

An alternative way to scale is to use some tricks (this allows not to change the box command):
```
draw 20x20 [pen off  fill-pen color  translate 0.5x0.5 [scale 2 2  box -0.5x-0.5 9.5x9.5]]
```

Or do all the calculations in one's mind and declare the matrix (again, the box command is unchanged):
```
draw 20x20 [pen off  fill-pen color  matrix [2 0 0 2 0.5 0.5] [box -0.5x-0.5 9.5x9.5]]
```

So the root of the issue here is that while the image (it's pixels) *mostly* lies in the 1st quadrant (QI) of the CS-2, some small parts of the image do extend into QII, QIII and QIV, and that extension amount depends on the scale chosen:

![](https://i.gyazo.com/9cd4bd491d3f1111964588d5a3ccaedf.png)

In CS-3 scaling works out of the box: `draw 20x20 [scale 2 2 ...]` or `draw 20x20 [(all coordinate pairs multiplied by 2)]`. And the image wholly belongs to QI.

### 4. Draw commands are inconsistent in their choice of CS.

Most of the Draw commands and View work with CS-2.

For example, `draw [pen off box 0x0 2x2]` draws a 2x2 box:
- from pixel 1x1 center (0x0 in CS-2)
- to pixel 3x3 center (2x2 in CS-2),
- affecting total area of 3x3 (-0.5x-0.5 to 2.5x2.5 in CS-2).

While some (image, clip) work with CS-3.

`draw [image img 0x0]` (suppose img/size = 2x2) draws:
- from pixel 1x1 upper left corner (-0.5x-0.5 in CS-2)
- to pixel 3x3 upper left corner (1.5x1.5 in CS-2)
- affecting total area of 2x2, as expected (but more interestingly, `scale N N image .. 0x0` still draws from the upper left corner on, regardless of N, proving that it works totally in CS-3 and not in CS-2)

### 5. Command format differences

`scale` does not accept a pivot point. It always scales around 0x0 in CS-2.

`rotate` does accept a pivot point (in CS-2 as an integer pair).

`transform` combines everything together (with all the pros and cons), but does not specify the order of operations.

## Summary of +/- of all CSs

CS-1.
Pros: no idea. Probably easier for beginners to accept.
Cons: noisy iteration.

CS-2.
Pros: good for lines of odd thicknesses.
Cons: bad for lines of even thicknesses, for scaling, for rotation, and simply unintuitive. Does not intersect with CS-1.

CS-3
Pros: good for lines of even thicknesses (including 0), for scaling, for rotation, easier to understand. CS-1 points refer to corners of pixels in CS-3.
Cons: bad for lines of odd thicknesses.

## Possible solutions

#### A. Let `rotate` and `scale` work in CS-3.

This is the least intrusive thing to do. Most of the problems will remain, but scaling and rotation will be simplified.

A1. Same as A, but also remove the `<center>` argument from rotate, let it assume 0x0, same as `scale`.

A2. Same as A, but also add an optional `<center>` argument to scale, defaulting to 0x0, same as `rotate`.

A1-A2 solve the discrepancy between `scale` and `rotate` formats.

#### B. Let `rotate` and `translate` accept non-integers, just as `scale` does.

This will break the current design, where one can assume that all pairs in the draw block are coordinates, and everything else is not. Once the floating point pair is introduced, this will become more feasible.

Most of the problems will remain, but rotation will be simpler.

#### C. Let all View/Draw commands work in CS-3.

Ridding of the trickiest CS (CS-2) eases the reasoning about coordinates. Scaling and rotation becomes simple. Breaks existing draw code.

#### C1. Same as C, but declare that an integer pair *means* a pixel center.

So that 0x0 *will mean* 0.5x0.5 inside the draw/view command blocks.
Or, putting it another way: pair! will refer to CS-2, but float-pair! - to CS-3.

Lines of both odd and even width will become aligned on pixel boundaries (provided one uses 0x0 for odd line widths, 0.0x0.0 for even ones).

More backward compatible than C.

However then a conversion of, say, 0x0 into 0.0x0.0, will alter the meaning and the outcome. An unwelcome effect.

And it's only possible if float-pair! will be a different datatype than the pair! itself.

#### C2. Same as C1, but also make CS-1 0x0-based.

Then the pixel referred to by 0x0 in a draw command will be the same pixel that is used in pick/poke commands, thus effectively eliminating CS-1 (it becomes CS-2). At least there will be uniformity between Draw commands and pick/poke.

#### C3. Same as C, but also make CS-1 0x0-based.

CS-1 will still refer to a corner of the pixel: top-left instead of bottom-right. Will ease the iteration though.

#### C4. Same as C, but let shape commands (box, polygon, circle...) allow to specify inner or outer points of the outline instead of centers (default)

For any shape (polygon/box/circle...):
![](https://i.gyazo.com/e0f91308fa944abe77220b8b3e5871be.png)

Mode 1 (default): specified points will designate the line center.

Mode 2 will tell that the specified points designate the outer margin of the shape.

Mode 3 will tell that the specified points designate the inner margin of the shape.

![](https://i.gyazo.com/3671b52f89b4fc00edbb7a3b2bd0bf38.png)

Mode 1 (default): specified points will designate the line center.

Mode 2 will tell that line should be drawn to the right of the vector.

Mode 3 will tell that line should be drawn to the left of the vector.

Mode 1 is reversible: `line 0x0 100x0` and `line 100x0 0x0` are equivalent. Modes 2 and 3 are reversed by each other: `line right 0x0 100x0` = `line left 100x0 0x0`.

This is one way to align the output to pixel boundaries. Regardless of line-width.

#### C5. C4 + C3.

Of course, more ideas how to deal with this are welcome!


### Helper func

This function has been very helpful to me in showing what draw is doing on a pixel level:
```
inspect: function [i] [
	pixel: 10x10
	? (also i': make image! i/size * pixel
	x: y: repeat y i/size/y [
		repeat x i/size/x [
			p: as-pair x y
			draw i' compose [
				line-width 1 pen (i/:p) fill-pen (i/:p)
				reset-matrix translate (pixel * (p - 1x1))
				box 0x0 (pixel - 2x2)
			]
		]
	])
	i'
]
```

E.g. `inspect draw 4x4 [line 0x0 3x0]`
