Here are some possible directions where we could go in order to support custom widgets:

1. Support "native" customization: allow native widgets to be customized by using only the OS API (Theme and VisualStyles API on Windows). 
    * Pro: the big work is done by the OS, still 100% native widgets.
    * Con: heterogeneous support across OSes.

2. Draw-based widgets: create a new set of widgets entirely using Draw dialect, and add to face object the necessary framework to handle states and their corresponding Draw blocks (or transformation functions acting on a single Draw block). 
    * Pro: fully cross-platform look'n feel.
    * Con: heavy work, not necessarily easy to customize.

3. Image-based widgets: use a set of predefined images for each state of each widget.
    * Pro: very simple, limitless graphic freedom.
    * Con: higher memory usage, harder to implement truly dynamic changes.

Also, it is possible to have a mix of 2 and 3. 

In all cases, the face object needs to be extended to handle different "states" and the logic to switch from one to the other.