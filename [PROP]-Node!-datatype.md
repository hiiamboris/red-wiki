----
**TL;DR - the proposal is at the end, the rest is introduction.**

Let's construct a graph in Red.

What data is required:
- a set of nodes (vertices); possibly unordered
- nodes are connected with arrows (edges)
- both nodes and arrows can hold a payload

# Naive model

## Naive tree (of arbitrary branching factor) example:

`[ "payload" [arrows] ]`
more expanded:
```
[
  "node payload"
  [
    "arrow payload"
    [
      "node payload"
      [more arrows]
    ]
    "arrow payload"
    [
      "node payload"
      [more arrows]
    ]
  ]
]
```
and so on... `[arrows]` is either empty or contains arrows pointing at descendant nodes.

How do we for example **move a node** from one place to the other?
We need to remove arrows pointing at it (there can be more than one!). Alas, our node has no info on incoming arrows, so we have to traverse the whole tree to do this step up. But how do we know, while traversing, that we're in the same place where a removal is requested? This same node can appear anywhere. Wow, bad luck.

## Okay, let's make this tree **more advanced**. It now has the info on ascending nodes:
```
root: [ 
  arrows-into-root: []      ;) as it's a root node - it has no ascendants
  "root node payload"
  arrows-from-root: [
    root
    "arrow payload"
    node1: [
      arrows-from-root
      "node1 payload"
      [more arrows]
    ]

    root
    "arrow payload"
    node2: [
      arrows-from-root
      "node2 payload"
      [more arrows]
    ]
  ]
]
```

So, now we still want to **move** a node. We check the arrows pointing at it and remove them all as

`forskip arrows 3 [if same? node third arrows [remove/part arrows 3]]`

Now we want to insert this node (branch) somewhere else, into `node-x` as

`repend node-x [node-x "payload" node]`

Oh noes! We forgot something! Our `node` contains `arrows-from-root`. Let's fix it:

`change/only node third node-x`

That was easy, since we moved the node completely. If we wanted to **copy** the node, we would have to append new arrows to the old ones.

But here's a **trick**. We may not know that my node has only one ascendant. Perhaps it's incoming arrows come from different nodes?
Then we'll have to traverse all the arrows and remove selectively.

Now what if we want to **find** a node with a certain **payload**? Uh-oh.. we need to traverse the whole tree:
```
look-in: func [node] [
  set [in-arrows payload out-arrows] node
  unless payload = what-I'm-looking-for [
    foreach [_ _ node] out-arrows [look-in node]       ;) recursive traversal
  ]
]
```

Okay, **scratch that**. It was just to show that simplest things become real ugly real fast.


## Note **shortcomings**:
1. **Exposed internal** structure. Exposed means:
- it's **fixed** - any change in it makes all previous code obsolete and in need of rewrite and bug hunt
- it's hard to work with - a lot of work is required to maintain the **integrity** of this structure
- everyone has to **reinvent** the wheels
- requires **numeric indexing** instead of the more readable, portable and extendable named path accessors (like /in, /out, /value)
2. Very **limited** functionality.
3. Painfully **slow**.

We can **remedy (1)** by providing a context full of functions to work with this structure and abstract from implementation.


**Optimizing lookups** by payload (3).

We can have a `map! of [payload -> [nodes with this payload]]`. And even better:
`map! of [payload -> [ [nodes with this payload] [arrows with this payload] ]]`. This will enable both arrow and node lookup.
Or even better, two **separate maps**, since it's likely we're looking either for a node or for an arrow but not for both.
Or we can use `hash! with [payload [node] payload [node] ...]` and another for arrows. Slower though.
Of course these structures will have to be constantly **kept in sync** with the tree itself.

But here is more...

How would e.g. **`foreach` know** that our node is not a flat list, and how would it know the structure of it? We'll have to have our **own iterator** functions.
Actions on series are not going to abstract the model, so we will also have to have our **own actions**.
**Think** all the boring stuff from **OOP**.

## Let's take another approach.

Starting with operations that a graph **should support**:
- add a `[src "payload" tgt]` arrow (possibly duplicating the same arrow multiple times...)
- list outbound arrows of `src` node: by mask `[src ... ...]`
- list  inbound arrows of `tgt` node: by mask `[... ... tgt]`
- remove a specific `[src "payload" tgt]` arrow only (once or multiple times)
- remove all `[src ... ...]` arrows
- remove all `[... ... tgt]` arrows
- remove all `[node ... ...]` and `[... ... node]` arrows at once - effectively removing the given node from graph
- read or change node's payload
- read or change arrow's payload
- list all nodes by payload
- list all arrows by payload
- list or visit all nodes, unordered, once (like `foreach` on maps)
- list or visit all nodes, ordered, without cycle prevention (e.g. for tree branches)
- list or visit all nodes, ordered, once (when it's known a graph can be cyclic, e.g. reactivity graph)
- reverse an arrow, in place or as a new arrow

Possibly also:
- list arrows by two attributes at once: `[src payload ...]`, `[src ... tgt]`, `[... payload tgt]`
- populate graph with nodes, then connect those with arrows... (unconnected nodes will then be known to the graph)

What should likely be done **separately**, on top of the graph functionality (if required):
- pathfinding algorithms
- select/extract a subgraph, compute inverse graph, or other fancy permutations


Also, we know that a **node** can have many arrows. We want it as a **reference** then, so it's not repeated. An object or a block would do.
An **arrow** on the other hand, can only be mentioned in two nodes, so making it an object/block is **not necessary**.
Moreover, we want to be able to **filter arrows** by their components - be it start node, end node, or payload.

Thus naturally emerges a **`hash!` of arrows**:
```
arrows: hash! [
  src-node "payload" tgt-node
  src-node "payload" tgt-node
  ...
]
```
where each node can be an `object! [value: "payload" out: [outbound arrows] in: [inbound arrows]]`.
We can select & return a sublist of arrows in the same format, filtered by some criteria.
We got **somewhat fast** lookups, although to build a filtered list of arrows (i.e. to get all those with given payload)
we still need to do multiple lookups and build a **new block of them each time**.

**Separate maps** can be added to **speed up** lookups:

`map! ["payload" -> [nodes with this payload]]` - where each node is an object (or block)

`map! ["payload" -> [arrows with this payload]]` - where each arrow is the `arrows` `hash!` `at` given arrow start
This last map can replace the `arrows` `hash!` if we dedicate a separate 3-item block for each arrow.

Note that although the naive tree structure has it's shortcomings, internally it still the same as long as nodes are blocks or objects.
E.g. in this new model, the root node object **mirrors the initial naive design**.

And though, here we can remove `out` and `in` fields from the node and still be able to get that list by parsing the `arrows` `hash!`,
by doing so we will sacrifice the speed at which we obtain a list of nodes's in/outbound arrows, which is likely unwanted.

What we did with this model is simply **compiled a few references** (a hash and 2 maps) for fast and convenient access.

Okay, we're happy with the internal structure now.

## Moving on to interface with other code...

The API may look roughly like this:
- `graph/nodes` - all nodes known - list of objects/blocks
- `graph/arrows` - all arrows known - just returns/copies the `arrows` `hash!`

Filtered (either using a map or constructing result blocks on the fly):
- `graph/arrows/from node` - only outbound arrows of node in format `[node "payload" ...]`
- `graph/arrows/into node` - only inbound arrows of node in format `[... "payload" node]`
- `graph/arrows/only src tgt` - only arrows of `[src ... tgt]` pattern
- `graph/arrows/with-value value` - only arrows of `[... value ...]` pattern
- `graph/nodes/with-value value` - only nodes of `[... value ...]` pattern

Modification:
- `graph/arrows/add src tgt value` - arrow is not an object, can be duplicated
- `graph/arrows/del src tgt value` - deletes first arrow found; can be done multiple times
- `graph/nodes/del node` - removes all arrows of `[node ... ...]` and `[... ... node]` patterns

Since these return lists of known 1- or 3- item format, shallow iteration is simple:
- `foreach node graph/nodes [...]`
- `foreach [src tgt val] graph/arrows [...]`

To put it another way, `graph` object provides **2 functions with refinements** that work on global hash and map structures.
Question of deep iteration remains unsolved so far.

It should also be said that graph functions should **act differently** based on graph type.
Graph can support various **options**:
- allow multiple equal (non-unique) arrows or not?
- allow nodes with the same/equal payload to coexist?
- allow arrows to have payload? (more RAM)
- treat arrows as directional or not?
- how many in- or out-bound arrows a node can have? (to put a limit; or if hundreds of arrows per node - use hashes to store them)
- permit cycles or not?
- permit self-referencing nodes (arrows from a node into itself) or not?
- type of payload (hashable or not) - can affect efficient indexing strategy
- if it's a tree, should it balance itself automatically? using what algorithm?
- what is the optimization target - fast node lookup? fast arrow lookup? removal? addition?

Based on these options, graph can contain **different code** and use different internal **model**.

Using **iteration** as an example:
- DAG nodes may be visited in their linear order
- tree may allow multiple visits to a shared branch, and not care about cycles
- cyclic graph may provide a guarantee that each node is visited only once (with some extra overhead)


## Actions?

How can we apply actions to this graph, and do we need to modify them in R/S or use ports?

Actions seemingly do **not really fit** the **non-linear** and/or unordered data model:
It makes sense to write `put graph node inner-node` to attach an `inner-node`.
Or `remove/key graph node` to remove all inbound arrows pointing at `node`.

But `get`, `append`, `insert`, `remove`, `move`, `change`, `clear` are positional.
They should accept not graph as a whole, but graph at some position, to be of use.

And `pick`, `find`, `select`, `at` - what these should return?

This brings us to a requirement for **self-similarity**: every position in a graph is a graph.
And this brings us to `node!` datatype. But the requirement for it to be a new datatype will be shown later.

## Automatic reflection

If arrow lists are returned as blocks, we need **changes** on those blocks to be **reflected** properly **on the graph**, so that it's **integrity** is ensured.
We can hold the arrows and nodes structures in an object with custom **`on-deep-change*`** function that will synchronously take care of that.
(or can we use `modify` to transfer ownership of those structures to the object, without actually putting them into it?)
We can not use deep-reactors for that, as those can be executed asynchronously and leave the graph in an invalid state.


## Node ID

A remark should be made on **why** an **object or block** is proposed to serve as **node unique identifier**.

Node ID has generally **to be unique** else we won't be able tell nodes apart.
A graph where every node's payload is unique can have the payload as node ID. But this is a big limitation.

But not just it has to be unique at certain point T in time.
It has to be **unique until the program finishes**, otherwise it's ID may remain somewhere,
and if we delete a node, then reassign it's ID to another node,
we'll certainly get an undefined behavior once we access this new node by this ID, thinking it's an old one.
We can produce IDs incrementally as integers, but Red has only 32-bit integers and it's easy to run out of 32-bit space like this.
The only datatype that can hold 48bit or larger integer is `tuple!`, which isn't suitable or convenient for the task.

**Object/blocks provide an easy way out:** they are represented by their memory address.
GC ensures that no references to such node remain when that address is allocated for new data. So, it is always unique.

Hashtables lookups use as keys:
- objects memory address directly
- hash of block address & block index

So it's a **fast O(1) lookup** as long as `find/same` is used.
**Maps could** provide the same kind of lookups, but currently do not allow their keys to be objects or blocks.
This is a point that can be improved, but not a showstopper.


## Now all looks fine, until we consider the RAM requirements.

Here are some empirical stats for Red containers of given length:

| type | length | bytes |
| ---  | ---    | --- |
| vector | 5 items  | ~55 |
| block | 1  items | ~32 |
| block | 3  items | ~70 |
| block | 6  items | ~120 |
| block | 10 items  | ~180 |
| block | huge N | 16*N |
| object | 0-4 words | ~260 |
| object | 5+ words | ~560 |
| hash | 2  items | ~650 |
| hash | 8  items | ~850 |
| hash | 10 items | ~910 |
| hash | huge N | 32*N |
| reactor | 8 items | ~5100 |

So if we were to construct just **1M nodes** (think an AST of a ~1MB file) as objects, without considering their in/out arrow lists or maps or hashtables,
**only nodes** will occupy **260MB!**

We can use **blocks** with accessors instead: `[value: [wrapped payload] in: [arrows] out: [arrows]]` - that's 120 bytes.
We have to wrap payload, to be able to type `node/out` or `node/in` even if payload is an `'in` or `'out` word - that's another 32 => to a total of **150MB** per 1M nodes.

We can shrink our blocks to just 3 items, using **numeric accessors** instead of `/in` `/out` and `/value` - an acceptable **100** bytes per node, but with ugly interface.
We can provide friendly accessors as functions: `inbound-of node`, `payload-of node`... But this will likely be ugly as well. Plus an unnecessary slowdown (function call overhead) of the most often used operations.


## Let's return to the `node!` datatype. How can it be implemented and it's benefits.

**Observe the commonality** between these data layouts:

- `[source payload target]` - general graph arrow
- `[[inbound arrows] payload [outbound arrows]]` - general bidirectional graph node

- `[previous value next]` - doubly linked list entry

- `[left payload right]` - binary tree node
- `[parent face! [pane]]` - possible VID faces tree node

- `[inputs relation outputs]` - one way to model reactivity: where relation is a node
- `[source-relation obj/path target-relation]` - an arrow in this reactivity model
- `[source-relation [obj/path event-stream] target-relation]` - event-stream or actor+message based reactivity arrow

- `[[source relations] obj/path [target relations]]` - another way
- `[input/path relation output/path]` - an arrow in this model

- `[sender message receiver]` - general message

- `[input weight output]` - artificial neuron connection

- `[left-expression op right-expression]` - parse tree (AST) infix operator (one of the exprs = none if it's unary)
- `[function 'func-apply [arguments list]]` - function application

- `[subject predicate object]` - Semantic triple: the core of RDF and associative databases.

We can model data flow, electronic cicruits, bloodline tree, whatever... only imagination is the limit.
All these tasks can be represented using this **single building block**... with only a **size of a `cell!`**:
```
red-node!: alias struct! [
  header  [integer!]
  left  [node!]
  value [node!]
  right [node!]
]
```
(do not get confused by R/S `node!` type (alias of `int-ptr!`) vs the proposed Red `node!` type (of `cell!` size)).

**Advantages** are:
- **tiny** RAM footprint - only 16 bytes per node (compared to 260 byte object)
- can support multiple **aliased accessor** names (`/in` = `/left` = `/prev` ...)
- native **foreach** and other HOFs support (even ports won't likely give that)
- HOFs can accept such a node instead of block and switch from shallow to **deep visiting** strategy, possibly dependent on graph type
(possibly `foreach/reverse` as well to negate the traversal direction)
- a symmetric building block used for both nodes and arrows of a graph (and it will be interesting to see what other applications will be found some day)
- not user-extendable in any way, so no need to check for block's length or if given words are present in the object

**Open questions** are:
- how `node!` will **support references** to scalars and other series (need to know their type, header), including other `node!`s. In other words, how to reliably reference another cell, and at the same time, keep the graph structure small? If that's not possible, we can still make it 4-cells (64byte) size, keeping all the benefits
- how to best test for **equality** and how to optimize it for faster **lookups**,
as having an arrow `[src value tgt]` one can look for a particular item in that block (say, `src`),
but a `node!` will have to be searched for as **a single value**
- what **actions to support** and how they map to operations on a graph (apart from obvious actions on returned lists)
- can node be **modified** or only **constructed**?
- if modifiable, relation with **`on-deep-change*`** (as it is meant to replace a block/object)





