<table>
  <tr>
    <td>REP:</td>
    <td>&lt;Leave Blank&gt;</td>
  </tr>
  <tr>
    <td>Title:</td>
    <td>&lt;REPL Enhancements&gt;</td>
  </tr>
  <tr>
    <td>Author(s):</td>
    <td>&lt;Neelesh Chandola @nc-x, [@x8x https://github.com/red/red/issues/1165 ]&gt;</td>
  </tr>
  <tr>
    <td>Status:</td>
    <td>Draft</td>
  </tr>
  <tr>
    <td>Date Created:</td>
    <td>&lt;Leave blank&gt;</td>
  </tr>
  <tr>
    <td>Date Last Actioned:</td>
    <td>&lt;Leave blank&gt;</td>
  </tr>
</table>

## Summary

The current REPL is extremely basic. The user experience can be improved by improving the autocompletions and line editing support. Also extremely useful would be syntax highlighting.

## Description

In the REPL, pressing TAB currently does not allow to cycle through the completions,i.e. it removes the "auto" from autocompletions. There should be a simple mechanism for the user to cycle forward or backward through the completions.

Currently, the REPL provides no way for the user to edit mistakes in previous lines of a multiline block, i.e. the user has to manually write it all again, leading to a poor UX. A simple key-combination which allows to move up to a previous line cyclically for editing and then come back to the current line without losing the already typed code would be highly appreciated.

## Use Case 

### Autocompletion Enhancements

Given:

```red>> compl```

Pressing TAB currently returns:

```
red>> complement complement? complete-from-path 
red>> compl
```

It would be better if *further* TAB pressed would cycle thru the proposed words until the ENTER key is pressed.

TAB should move in a cyclic way and should come back to start once all the autocompletions have been shown and continue the chain.

On pressing ENTER a SPACE char be auto added after the selected word.

In the above case the final output will therefore be 

```
red>> compl
TAB:
red>> complement complement? complete-from-path 
TAB TAB TAB TAB ENTER:
red>> complement 
```

Also required would be a new key combination to cycle backwards (Maybe CTRL+TAB). This would be especially useful if by mistake you pressed an extra TAB and if you do not want to waste time to go through the whole cycle.

### Line Editing 

The current behaviour of the REPL is that once you press ENTER, you move to the next line and cannot edit the previous line.

```
sum: function [
    a [integer!]
    b [integre!]
] [
    print [a + b]
]
```

If you type the above code in the console, you cannot go up and edit the mistake.
Therefore, there should be a key combination - (CTRL + up-arrow) to move to the upper lines without losing the code on the current line (and (CTRL + down-arrow) to move to the next line) and continue editing.

### Syntax Highlighting

There should be an optional (hidden behind a switch) syntax highlighting support in the REPL.

## Benefits

Easy to use REPL would lead to better UX and will be helpful for beginners to get started with Red.

## Consequences

None that I can think of.

## Assistance

Wait patiently while someone implements the REP. :P