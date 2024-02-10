# Introduction
CSV codec implements CSV/DSV parser and emiter for Red language. It implements
[RFC 4180](https://tools.ietf.org/html/rfc4180) support with extension to support non-standard data.

## Supported data formats
Before explaining how each function works, let's have a look at how a table
can be represented in Red. We will start with a simple table:

```
Name    | Mass  | Orbit
-----------------------
Mercury | 0.055 | 0.4
Venus   | 0.815 | 0.7
Earth   | 1.0   | 1.0
Mars    | 0.107 | 1.5
```

This table can be represented in Red in more different ways. The simplest one
is `block!`:

```
["Name" "Mass" "Orbit" "Mercury" 0.055 0.4 "Venus" 0.815 0.7 "Earth" 1.0 1.0 "Mars" 0.107 1.5]
```

I intentionally haven't formatted the block, so it's obvious that without
additional knowledge of record length, it's hard to guess it. This leads us to
second solution: `block!` of `block!`s:

```
[["Name" "Mass" "Orbit"]["Mercury" 0.055 0.4]["Venus" 0.815 0.7]["Earth" 1.0 1.0]["Mars" 0.107 1.5]]
```

This nicely represents the original table and is probably closest to CSV data also.
This is the default output of `load-csv` function. However there's one problem,
it's not obvious if first `block!` is header or record. Which brings us to
third solution: `block!` of `map!`s (or `object!`s, if you prefer), called `as-records`
in `load-csv`:

```
[
	#[Name: "Mercury" Mass: 0.055 Orbit: 0.4]
	#[Name: "Venus" Mass: 0.815 Orbit: 0.7]
	#[Name: "Earth" Mass: 1.0 Orbit: 1.0]
	#[Name: "Mars" Mass: 0.107 Orbit: 1.5]
]
```

This format is very obvious and self-explanatory. However also very redundant.
Which brings us to last solution, `map!` (again, or `object!`) of columns,
called `as-columns` in `load-csv`:

```
#[
	Name: ["Mercury" "Venus" "Earth" "Mars"]
	Mass: [0.055 0.815 1.0 0.107]
	Orbit: [0.4 0.7 1.0 1.5]
]
```

The redundancy here is much lower but at the cost that you need to write
special function to get one record.
As shown above, each format has its advantages and disadvantages. That's why
it makes sense to support them all and let the user decide which one to use
while using sane default that's most similar to CSV data structure. Choosing
the right format depends on a lot of circumstances, for example, memory usage -
column store is more efficient if you have more rows than columns. The bigger
the difference, the more efficient.

Bear in mind that the example above uses rich data types in the output, but the CSV
codec doesn't do that itself. In order to keep the codec small and focused, it only
parses the data into chunks of text and puts those into structures. Converting them
to Red datatypes is beyond the scope of the codec, and is handled by other functions
operating on the structures the codec produces.

## LOAD-CSV

`load-csv` decodes `string!` of CSV data to Red `block!` or `map!`, depending on
the mode. Usage is simple:

	load-csv csv-data

There are some refinements to modify function behavior:
* `/with delimiter` - Use different delimiter. Default is a comma (`,`).
* `/header` - Treat the first line as a header. Will return `map!` with columns as keys.
* `/as-columns` - Return `map!` of columns. If `/header` is not used, columns are named
automatically from A to Z, then from AA to ZZ, etc. `/as-columns/header` is the same as
`/header`.
* `/as-records` - Return `block!` of `map!`s. Keys are named by columns using the same
rules as with `/as-columns` refinement.
* `/flat` - Return flat `block!` instead of `block!` of `block!`s.
* `/trim` - Ignore spaces between quotes and delimiter.
* `/quote qt-char` - Use different quotes character. Default is the double quote (`"`). 

Some refinements cannot be used together, for example `/as-columns` and `/as-records`. If you
use an invalid combination of refinements, `load-csv` will throw an error.

If you want the default format, but to separate the header, this is an easy way to do it.
```
>> s: {a,b,c^/1,2,3^/4,5,6}
== "a,b,c^/1,2,3^/4,5,6"
>> data: load-csv s
== [["a" "b" "c"] ["1" "2" "3"] ["4" "5" "6"]]
>> hdr: take data
== ["a" "b" "c"]
>> data
== [["1" "2" "3"] ["4" "5" "6"]]
```

The rationale for `/header`'s behavior is that it means it should be *used*, not just that it *exists*. 

## TO-CSV

`to-csv` encodes `block!`, `map!` or `object!` and returns `string!` of CSV data.
There are some refinements to modify function behavior:

* `/with delimiter` - Use different delimiter. Default is a comma (`,`).
* `/skip size` - Treat `block!` as table of records with `size` fields.
* `/quote qt-char` - Use different quotes character. Default is the double quote (`"`).