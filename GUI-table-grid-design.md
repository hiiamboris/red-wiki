# Notes from HenrikMK

My table widget is R2 specific, and I had a month to come up with the fastest possible table solution for the software we're building. What I know is that building everything from faces is typically not the fastest way to do it. In R2's case, it's very far from the fastest way.

The first design requirement was to build a table that could manage any size of data and work as fast as possible with scrolling, selection and tool-tips, so the starting point was to build the table grid to never process any kind of data that isn't going to be displayed now. That means building per-cell data picking functions and functions that calculate all dimensions and visible parts with pixel accuracy without drawing anything on screen.

The second design requirement was that everything is offline until you call REDRAW, something that is a problem with RebGUI's TABLE widget. REDRAW should never do any kind of calculations on sizes, offsets, content formatting, etc. It just reads all settings and dimensions and draws the table for you, once.

RebGUI unfortunately has a tendency to make REDRAW do too much, and requires redrawing the UI to get new sizes and offsets. This puts annoying limitations on its resize system, where you cannot inform certain parts of a UI about dimensions without specifically drawing other parts. For example, if you have a tab panel, UI elements in hidden panels do not have the correct size and offset, until the panel is drawn. That means, if I want to use the size and offset of a hidden UI element in another tab, I can't do that without calculating and setting the size manually, bypassing RebGUI's resizing system. This is the wrong way to do it. Don't do that with Red.

A third design requirement was to not have the table widget ever do any filtering, sorting or editing of data on its own. This should all be worked in with handlers, so we don't need to have the table widget redrawn, if we want to export data sorted and filtered in a particular fashion, and also avoid bloating the widget with sophisticated sorting methods.

A fourth design requirement is the ability to store and apply table states for undo/redo, and allow saving and retrieving them from disk, if needed. This goes back to the REDRAW issue. So the table state is a list of all properties required to configure the table for a particular display. Each property is settable or retrievable via standard functions available through its API (which does not have a dialect at all yet). If you need to do any kind of face hacking or probing the structure of the face tree, then there's a function missing. Once all properties are set up, call REDRAW once, and the table is correct.

A fifth design requirement is that row and column references are never physical. This was an infuriating part of RebGUI's TABLE, until some functions were added to clunkily convert physical row IDs into logical row IDs. In 99.9% of cases, you want to get and set selection data from sorted row and column order, not from the physical order.

I may release the source for it at some point, when the table widget is integrated into our program and depending if I'm allowed to do that.
