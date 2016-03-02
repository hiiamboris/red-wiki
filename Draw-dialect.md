Table of Content:
* [Abstract](#abstract)
* [Commands](#commands)
 * [Line](#line)
 * [Triangle](#triangle)
 * [Box](#box)
 * [Polygon](#polygon)
 * [Circle](#circle)
 * [Ellipse](#ellipse)
 * [Arc](#arc)
 * [Curve](#curve)
 * [Spline](#spline)
 * [Image](#image)
 * [Text](#text)
 * [Font](#font)
 * [Pen](#pen)
 * [Fill-pen](#fill-pen)
 * [Line-width](#line-width)
 * [Line-join](#line-join)
 * [Line-cap](#line-cap)
 * [Anti-alias](#anti-alias)
* [Default values](#default-values)
* [Sub blocks](#sub-blocks)
* [Source position](#source-position)
* [Draw function](#draw-function)

# Abstract

Draw is a dialect (DSL) of Red language, which purpose is to provide a simple declarative way to specify 2D drawing operations. Such operations are expressed as lists of ordered commands (using blocks of values), which can be freely constructed and changed at run-time.

Draw blocks can be rendered directly on an image using the `draw` function, or inside a View face using the `draw` facet (see [View documentation](https://github.com/red/red/wiki/Red-View-Graphic-System)).

# Commands

Commands can be drawing instructions or options/modes setting for the drawing instructions. When a mode is set, it will affect all subsequent operations in the current Draw session.

Most drawing commands require coordinates to be specified. The used 2D coordinate system is:
* x axis: increasing from left to right of the display.
* y axis: increasing from up to bottom of the display.

*(insert graphic here)*

## Line

**Syntax**

    line <point> <point> ...
    
    <point> : coordinates of a point (pair!).
    
**Description**

Draws a line between two or more points.

## Triangle

**Syntax**

    triangle <point> <point> <point>
    
    <point> : coordinates of an edge point (pair!).
    
**Description**

Draws a triangle from the three edges provided.

## Box

**Syntax**

    box <top-left> <bottom-right>
    box <top-left> <bottom-right> <corner>
    
    <top-left>     : coordinates of the top-left of the box (pair!).
    <bottom-right> : coordinates of the top-left of the box (pair!).
    <corner>       : (optional) radius of the arc used to draw a round corner (integer!).
    
**Description**

Draws a box using the top-left (first argument) and bottom-right (second argument) edge points. An optional radius can be provided for making the corners round.

## Polygon

**Syntax**

    polygon <point> <point> ...
    
    <point> : coordinates of an edge point (pair!).
    
**Description**

Draws a polygon using the provided edges. The last point does not need to be the starting point, an extra vertice will be drawn anyway to close the polygon. Minimal number of points to be provided is 3.

## Circle

**Syntax**

    circle <center> <radius>
    circle <center> <radius-x> <radius-y>
    
    <center>   : coordinates of the circle's center (pair!).
    <radius>   : radius of the circle (integer!).
    <radius-x> : (ellipse mode) radius of the circle along X axis (integer!).
    <radius-y> : (ellipse mode) radius of the circle along Y axis (integer!).
    
**Description**

Draws a circle from the provided center and radius values. The circle can be distorted to form an ellipse by adding an optional integer indicating the radius along Y axis (the other radius argument becomes the X radius then).

## Ellipse

**Syntax**

    ellipse <center> <radius>
    
    <center> : coordinates of the ellipse's center (pair!).
    <radius> : radiuses of the circle (pair!).
    
**Description**

Draws an ellipse from the provided center and radius values. The second pair argument combines radius along X and Y axes using a pair! value. 

_Note_: `ellipse` provide a more compact way to specify an ellipse compared to `circle`.

## Arc

**Syntax**

    arc <center> <radius> <begin> <end>
    arc <center> <radius> <begin> <end> closed
    
    <center> : coordinates of the circle's center (pair!).
    <radius> : radius of the circle (integer!).
    <begin>  : starting angle in degrees (integer!).
    <begin>  : ending angle in degrees (integer!).
    
**Description**

Draws the arc of a circle from the provided center and radius values. The arc is defined by two angles values. An optional `closed` keyword can be used to draw a closed arc using two lines coming from the center point.

## Curve

**Syntax**

    curve <end-A> <control-A> <end-B>
    curve <end-A> <control-A> <control-B> <end-B>
    
    <end-A>     : end point A (pair!).
    <control-A> : control point A (pair!).
    <control-B> : control point B (pair!).
    <end-B>     : end point B (pair!).

**Description**

Draws a Bezier curve from 3 or 4 points:
* 3 points: 2 end points, 1 control point.
* 4 points: 2 end points, 2 control points.

Four points allow more complex curves to be created.

## Spline

**Syntax**

    spline <point> <point> ...
    spline <point> <point> ... closed
    
    <point> : a control point (pair!).

**Description**

Draws a B-Spline curve from a sequence of points. At least 3 points are required to produce a spline. The optional `closed` keyword will draw an extra segment from end point to start point, in order to close the spline.

_Note_: 2 points are accepted, but they will produce only a straight line.

## Image

**Syntax**

    image <image>
    image <image> <top-left>
    image <image> <top-left> <bottom-right>
    image <image> <top-left> <top-right> <bottom-left> <bottom-right>
    image <image> <top-left> <top-right> <bottom-left> <bottom-right> <color>
    image <image> <top-left> <top-right> <bottom-left> <bottom-right> <color> border
    
    <image>        : image to display (image! word!).
    <top-left>     : (optional) coordinate of top left edge of the image (pair!).
    <top-right>    : (optional) coordinate of top right edge of the image (pair!).
    <bottom-left>  : (optional) coordinate of bottom left edge of the image (pair!).
    <bottom-right> : (optional) coordinate of bottom right edge of the image (pair!).
    <color>        : (optional) key color to be made transparent (tuple! word!).
    
**Description**

Paints an image using the provided information for position and width. If the image has positioning information provided, then the image is painted at 0x0 coordinates. A color value can be optionally provided, it will be used for transparency. 

_Note_: 
* Four points mode is not yet implemented. It will allow to stretch the image using 4 arbitrary-positioned edges.
* `border` optional mode is not yet implemented.

## Text

**Syntax**

    text <position> <string>
    
    <position> : coordinates where the string is printed (pair!).
    <string>   : text to print (string!).

**Description**

Prints a text string at the provided coordinates using the current font. 

_Note_: If no font is selected or if the font color is set to `none`, then the pen color is used instead.

## Font

**Syntax**

    font <font>
    
    <font> : new font object to use (object! word!).

**Description**

Selects the font to be used for text printing. The font object is a clone of `font!`.

## Pen

**Syntax**

    pen <color>
    
    <color> : new color to use for drawing (object! word!).

**Description**

Selects the color to be used for line drawing operations.

## Fill-pen

**Syntax**

    fill-pen <color>
    fill-pen off
    
    <color> : new color to use for filling (object! word!).

**Description**

Selects the color to be used for filling operations. All closed shapes will get filled by the selected color until the fill pen is set to `off`.

## Line-width

**Syntax**

    line-width <value>
    
    <value> : new line width in pixels (integer!).

**Description**

Sets the new width for line operations.

## Line-join

**Syntax**

    line-join <mode>
    
    <mode> : new line joining mode (word!).

**Description**

Sets the new line joining mode for line operations. Following values are accepted:
* `miter` (default)
* `round`
* `bevel`
* `miter-bevel`

*(include graphic)*

## Line-cap

**Syntax**

    line-cap <mode>
    
    <mode> : new line cap mode (word!).

**Description**

Sets the new line's ending cap mode for line operations. Following values are accepted:
* `flat` (default)
* `square`
* `round`

*(include graphic)*

## Anti-alias

**Syntax**

    anti-alias <mode>
    
    <mode> : `on` to enable it or `off` to disable it.
    
**Description**

Turns on/off the anti-aliasing mode for following Draw commands.

_Note_: Anti-aliasing gives nicer visual rendering, but degrades performances.

# Default values

When a new Draw session starts, the following default values are used:

Property | Value
----- | --------
background	|  `white`
pen color	| `black`
filling		| `off`
anti-alias	| `off`
font		| `none`
line width	| `1`
line join	| `miter`
line cap	| `flat`

# Sub blocks

Inside Draw code, commands can be arbitrarily grouped using blocks. Semantics remain unchanged, this is currently just a syntactic sugar allowing easier group manipulations of commands (notably group extraction/insertion/removal). Empty blocks are accepted.

# Source position

Set-words can be used in the Draw code **in-between** commands to record the current position in Draw block and be able to easily access it later.

_Note_: if the Draw block length preceeding a set-word is changed, the recorded position is not updated.

# Draw function

It is possible to render a Draw block directly to an image using the `draw` function.

**Syntax**

    draw <size> <spec>
    draw <image> <spec>
    
    <size>  : size of a new image (pair!).
    <image> : image to use as canvas (image!).
    <spec>  : block of Draw commands (block!).

**Description**

Renders the provided Draw commands to an existing or a new image. The image value is returned by the function.
