# TABLE STYLE DESIGN

What's a table? And what features it should support? Let me list a few common examples.

1. There's old school **text table** - a few text-lists placed in parallel. It mirrors CSV - a few columns with headers, huge (but limited) number of rows.
<br>Basic requirements:
- strings and numbers display
- have column headers
- sortable by any column
- custom alignment for each column
- support of data editing, copy, paste
- adding/removing rows at run time
- tab & arrow navigation
- ideally, columns can be dragged around
- ideally, columns can be added/removed by the user (showing same data set, just different properties of it)

2. There's same thing but can display not just text, but **mixed data** in each cell, e.g. icon + name.
<br>Requirements:
- read-only display of images, blocks of images, blocks of mixed data
- custom renderers that would control how to render each data format (or on per-column basis)
- interaction with each item: should handle clicks
- ideally, editable data within those mixed blocks should stay editable (may not be worth it, at least natively)

3. There's a **2D table**:
- rows should have headings
- table can be sorted by a row, not only column

4. There's **spreadsheet**:
- persistent relative & absolute links to it's cells
- autoupdate graphs
- formulas, i.e. cell contents <> displayed (formatted) output
- different formats support (user-controlled?)
- cells can be unified or split again
- support of overlaid images (plots) that may not even align to cell margins

5. There are **document-like tables** - where there are still columns and rows. 
- each cell may span many rows & columns
- text can flow vertically (esp. in column headers)
- some cells in the header may be split diagonally, e.g. different titles for row and column headings

6. There's MS Access-like **forms** that map each screen to a single row returned by the DB. [See this](https://i.gyazo.com/b1e9533ed157541d72dbb06d0017b9fc.png). 
- DB brings about infinite (not feasible to retrieve fully) data: whether laid out into forms or just as text table
- DBs only support text and numbers, right? Maybe some convertible to string types, but no images/objects.. none of the Red richness
 <br> <br> I think this has nothing to do with table layouts though. It's a normal layout that gets updated using a DB request. As such, it makes me question the need for infinite data support.

7. Then there's **grid layout** - basically, a rectangle gets divided by N rows and M columns, and any face can occupy a single cell 1x1 or span multiple cells (e.g. 2x4).
[See this](https://i.gyazo.com/f57f827a839510e6dd8b7dc1884f8bc3.gif) and [also this](https://i.gyazo.com/16f31da799f363179b8bac731c39adbe.gif). 
- define grid elements by their row+column coordinates and span
- same during run time: ability to remove/add grid elements the same way
- no sorting here I guess, not addition of rows/columns, and generally it's a much less dynamic structure than a table
- cells likely are not data but faces
- each row/column can be resized: grid elements will follow
 <br> <br> In the grid, what *is* a row, or a column? How is it defined? What if some items are draggable? Do they belong to a single row/column or many? How do they move from one row/column to another?
 <br> <br> My argument against this is that VID is already pretty good at laying out stuff that the need in Red is tiny. I also think this has nothing to do with tables, but something VID should learn from: dynamic placement of draggable items is quite cool. Dynamic layouts niche is still empty in Red space.

8. There's even **table of tables**, or grid of tables... Table where each cell is also a table. Not sure how useful it is though, but interesting. [See this](https://i.gyazo.com/ca5fee1e273e734c52bb13071565f21c.jpg)

## Showcase

A few examples I have found on the web, to study.

Just a 2D table in a spreadsheet. In a View layout, column and row headers should be equal in their rights. Something like that will be useful in Reactivity explorer: 2D and dynamic. Sortable by column or row.
![](https://camo.githubusercontent.com/c00fe1a57f0d2c8ea5ac4c4ad91750a999810dd2/68747470733a2f2f692e6779617a6f2e636f6d2f33663062323863366662303036323338633239663131366161353931303164362e706e67)

---
Text table, sortable. Even/odd rows visually distinct. But header contains custom filter controls. Means to implement one either makes headers compound faces, or we allow reserving some rows after the header row (one in this case). Strange that filter row is not vertically compact: has big margins of doubtful significance.
![](https://i.gyazo.com/e2473a94fc0074f2aeaf84d78221b563.png)

---
Mixed table: text (colored, underlined, stricken through), images, input fields, buttons, checkboxes. No visible margins between columns. Uses text wrapping. Header contains a clickable checkbox. But most interesting is the separator line, or it looks like a comment under the 1st row - which is not divided by columns. 
![](https://i.gyazo.com/d7f6e90625448fedc8d7efe4afc04621.png)

---
Another mixed table. Multi-level header with cells spanning multiple columns. Other than that, nothing fancy: custom renderers for percents, blocks of icons, icon + text, also passive checkboxes. Filter on the top assumes real time filtering and update of table, and that means some performance / responsiveness. Horizontal scrollbar doesn't make much sense to me - probably shortened to take up less screen space? Or first 3 columns are sort of "pinned", and it scrolls around only the rest?
![](https://i.gyazo.com/20a209a90c88085127207c30c4d73406.png)

---
Mixed table again: text, icons, progress bars. Sortable both ways by column. Cell color depends on the cell value. Editable! Addition/removal of rows; interface that controls whether the row data is displayed as a result or as an editable control: drop-down, field. This echoes to spreadsheets a bit. Note also ellipses.
![](https://i.gyazo.com/cdcf1ee79102630b9d116a255d91ddb5.png)

---
Mixed table, content is simple, but: has skinned columns and cell separators.
![](https://i.gyazo.com/f7a9b36626209ab5caeadd8313a0b9fc.jpg)

## Features list

Anything missing here?

**Rendering**
- cell text/face alignment (left, right, top, bottom)
- cell text wrapping (table-wide or column-wide flag)
- cell text ellipsization (table-wide or column-wide flag)
- faces inside a cell (or at least fake faces)
- common and user-provided formatters for cell data (based on data format, or per column, or for each cell)
- common and user-provided renderers (based on data format, or per column, or for each cell)
- overloadable (replaceable) cell style, so that one can control whether cell is a real face, a gob, or just a virtual descriptor with Draw commands

**Layout**
- controllable (on a show/hide level) column & row separators (also just the size of empty spacing)
- user-defined separators (for styling/skinning)
- scrollers - part of the table (more work) or external (only good for small tables)?
- "pinned" columns/rows (including headers), unaffected by the scrollers
- synchronized scrolling of 2 tables? probably too much work: will require tables to coordinate their row heights - easier to unify 2 tables into one
- cells spanning multiple columns and/or rows
- named column groups - automatically span multiple columns, require a multi-line header

**Editing**
- editable and read-only modes for a cell (row, column) that can be switched on the fly (e.g. formula / formatted result)
- editable input validation by type
- user-provided editable input validators (per column, per cell)
- validators show hint text in case the input is wrong
- undo/redo per Henrik's notes? I'm not convinced - I think the encapsulating program should deal with this

**Sorting**
- sort by column
- reverse sort by column
- control over what columns can be used for sorting
- sort by rows
- reverse sort by rows
- automatically sorted row insertion ? (e.g. slow & weak DB connection, rows getting shown at the time they arrive) And how will this play together with editable cells (that would make rows jump around with each keystroke)

**Filtering**
- filters for each column
- chains of filters (`or`-ed/`and`-ed)
- embedded UI to leverage these filters - [example](https://www.ag-grid.com/)

**Interaction**
- tab navigation forth and back (row then column, or column then row) - requires an ability to scroll to a specific cell, and exposure of table layout to the navigation algorithm, including which cells are editable
- columns resize
- columns reodering by dragging
- addition of new columns by the user (then data must define the possible set)
- rows resize (spreadsheets only?)
- rows reodering by dragging ?
- addition of new (empty) rows by the user
- box-like selection of a range of cells, with export capability

**Description**
- would be nice to leverage VID: `table [column 100 "Header 1" [cell "this" cell "that"] ... column 200 "Header 2" ...]` (that's what I'm able to do [here](https://gitlab.com/hiiamboris/red-mezz-warehouse/-/blob/master/table.red), but makes the whole table quite heavy)
- table-wide cell addressing, like in spreadsheets - should we have a word for each cell (e.g. `my-table/A1`) or that's too heavy/useless? if not, how to refer to cells when calling a table function? as a string (e.g. "A$1")? in any case, that's better than `table/pane/N/pane/M` that might not even exist.

**Nonspatial geometry (events)**
- cell height balancing to ensure all cells in a row are aligned - after every change
- ability to map or bind full table, single column/row or single cell to some data source: renew table when data updates, change data when user changes the cell contents ('map' maps to a place in a block/object, 'bind' - associates with a certain word)
- user provided event handlers (per table, per column, per cell)
- embedded update cycles detection, like in spreadsheets?
- do we need infinite data support? and how to sort and scroll it?

Hard part of cell **height balancing**: how to make it efficient? E.g. I'm filling the row and I'm adding a 20x20 cell into column 1, then adding a 40x40 cell into column 2 (same row), then 60x60 cell into column 3. How many times row height will be refreshed? Naive implementation would visit each column on each addition, i.e. N^2 cell scans, and up to N row height updates (in this example after each new cell gets added). This will be slow and will look ugly.

Another issue with height balancing is separtion of knowledge. Would be best if cell had no idea about columns, rows or tables, and balancing logic totally lied in the table code. But how, if it's the cell who receives events?

One more issue: if some row gets resized at the top of the table, that means *all* the rows below should be relocated. If they have absolute coordinates, or if they are real face, this is an expensive operation.

Hard part of **mapping & binding**: Red object ownership system doesn't generate any on-change events for non-owned data, and especially for changes of bound word data. One solution would be to make a separate data object for each table, and this object will track it's changes and report them to the table UI. Another solution (slow) - scan all data periodically on timer and update.

Another problem is that any series assigned to table data becomes owned by the table, thus preventing user-defined deep reactions on that data.

So should table hold a separate owned data array? Or is it possible to map it directly to other data structures?<br>
How should data flow from the source into the data array and into the table layout? Should there be a separate data descriptor structure? Or should each user define his own functions that do all that?

This also concerns the `table!` datatype: what features will it provide and how the table style leverages them?

## Design questions

The main questions are:
- what minimal blocks of functionality should we provide to support the widest range of applications?
- how do we choose these blocks to best isolate one function from another?
- what ready styles should be combined from these blocks and shipped with Red? and what left out?
- how should the event flow look like?
- what extra operations, not UI-related, should the table provide? e.g. query a filtered set of cells, so that one may process them, or get data from... Or should this be beyond the style's functionality and part of the `table!` datatype?

Obviously, the more functionality we want to have the more work it will require for the user to work with such table. One thing I would certainly like to have is a style that you drop the data (object/block/whatever) to and it displays it without any further tweaks.

**How to organize the table as a tree?**
- only table: rows, columns, cells are accessed by table functions and given address (pair)
- each cell is an object, row & column selectors listing those objects
- table includes rows, rows include cells
- table includes columns, columns include cells (most natural, CSV-like, layout?)

Some thoughts:
- The benefit of having cell (as object) is the ability to control each cell's features individually. If it's a face - it leverages all the boons that View provides (and all the bugs..)
- The benefit of having column (as face/object) - can be resized or dragged around in one operation, can be styled individually, headers can be naturally defined. Also column then can be used without a table - and that makes text-list almost obsolete.
- The benefit of having row (as face/object) - ease of row height balancing, of odd/even row coloration, of row-oriented tab navigation.
- We can't have both columns and rows in the hierarchy at once. Or can - if they are not panels but virtual faces or custom objects, but that will require extra bookkeeping to keep both in sync.
- If columns/rows are faces (panels), then cell won't be able to span a few, and that's quite limiting.

**On event flow.**

OMG this is messy. Will depend on the tree structure choice of course. But the problem is that often we want things to update both ways: cell property is changed - update the table, table property is changed - update cells. But that just won't work, as it will branch events in geometric progression, and reactivity will simply ignore most of the events, and we won't even know how to debug this mess and why nothing gets updated. Also mind myriads of event-related bugs. So, this fragile thing requires veeeery careful design.

To scratch the problem surface...<br>
Example from my table style. I need cell renderers to be able to define their own cell sizes. At the same time I want cells to have column width, and each cell relies on the width to render it's content (e.g. to limit the number of chars to display). This is already a problem because when the size changes there's no way to know which (X or Y) coordinate did change.<br>
Then more... I need to balance rows, that is row should be as high as the tallest cell in it. So cell height affects row height. But the opposite is also true: when the row grows, cell content may wrap itself or otherwise adapt (images will change scale). And then after it adapted, what if the tallest cell in the row became small again, how to contract the row back? This will certainly require cell renderers provide not only size of the canvas, but size constraints, and let the table decide how to fulfill those best.<br>
Then columns. Table should initially size itself to fit all the columns. But later, if table is resized, columns should scale with it.


# Notes from HenrikMK

My table widget is R2 specific, and I had a month to come up with the fastest possible table solution for the software we're building. What I know is that building everything from faces is typically not the fastest way to do it. In R2's case, it's very far from the fastest way.

The first design requirement was to build a table that could manage any size of data and work as fast as possible with scrolling, selection and tool-tips, so the starting point was to build the table grid to never process any kind of data that isn't going to be displayed now. That means building per-cell data picking functions and functions that calculate all dimensions and visible parts with pixel accuracy without drawing anything on screen.

The second design requirement was that everything is offline until you call REDRAW, something that is a problem with RebGUI's TABLE widget. REDRAW should never do any kind of calculations on sizes, offsets, content formatting, etc. It just reads all settings and dimensions and draws the table for you, once.

RebGUI unfortunately has a tendency to make REDRAW do too much, and requires redrawing the UI to get new sizes and offsets. This puts annoying limitations on its resize system, where you cannot inform certain parts of a UI about dimensions without specifically drawing other parts. For example, if you have a tab panel, UI elements in hidden panels do not have the correct size and offset, until the panel is drawn. That means, if I want to use the size and offset of a hidden UI element in another tab, I can't do that without calculating and setting the size manually, bypassing RebGUI's resizing system. This is the wrong way to do it. Don't do that with Red.

A third design requirement was to not have the table widget ever do any filtering, sorting or editing of data on its own. This should all be worked in with handlers, so we don't need to have the table widget redrawn, if we want to export data sorted and filtered in a particular fashion, and also avoid bloating the widget with sophisticated sorting methods.

A fourth design requirement is the ability to store and apply table states for undo/redo, and allow saving and retrieving them from disk, if needed. This goes back to the REDRAW issue. So the table state is a list of all properties required to configure the table for a particular display. Each property is settable or retrievable via standard functions available through its API (which does not have a dialect at all yet). If you need to do any kind of face hacking or probing the structure of the face tree, then there's a function missing. Once all properties are set up, call REDRAW once, and the table is correct.

A fifth design requirement is that row and column references are never physical. This was an infuriating part of RebGUI's TABLE, until some functions were added to clunkily convert physical row IDs into logical row IDs. In 99.9% of cases, you want to get and set selection data from sorted row and column order, not from the physical order.

I may release the source for it at some point, when the table widget is integrated into our program and depending if I'm allowed to do that.


# References

- https://www.ag-grid.com/
- https://www.microsoft.com/en-us/research/uploads/prod/2020/04/extended_gridlets_esop.pdf