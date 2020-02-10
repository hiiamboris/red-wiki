_Copied from this [Bug report](https://github.com/red/red/issues/1983#issuecomment-225108511)_

<p>First thing to do is to compile the above code in debug mode <code>-d</code> option) or re-compile a GUI console version in debug mode with <code>do/args %red/red.r "-d -r red/environment/console/gui-console.red"</code> Many assertions and debug logs through the Red runtime code are not available in release mode.</p>

<p>For the first crash, I went straight to the <code>type?</code> native source in <code>%runtime/natives.reds</code> and put some debug logs there:</p>

<pre><code>    type?*: func [
        check?   [logic!]
        word?    [integer!]
        return:  [red-value!]
        /local
            dt   [red-datatype!]
            w    [red-word!]
            name [names!]
    ][
        #typecheck [type? word?]
probe "in type native"
        either negative? word? [
            dt: as red-datatype! stack/arguments        ;-- overwrite argument
            dt/value: TYPE_OF(dt)                       ;-- extract type before overriding
            dt/header: TYPE_DATATYPE
            as red-value! dt
        ][
            w: as red-word! stack/arguments             ;-- overwrite argument
stack/dump          
            name: name-table + TYPE_OF(w)               ;-- point to the right datatype name record
?? name
dump4 name
dump4 name/word
            stack/set-last as red-value! name/word
        ]
    ]
</code></pre>

<p>Once compiled and run in debug mode, I saw the crash happening at <code>dump4 name/word</code> with following output:</p>

<pre><code>Hex dump from: 00000000h

00000000:
*** Runtime Error 1: access violation
*** in file: /C/dev/Red/system/runtime/debug.reds
[...]
</code></pre>

<p>Looking at the previous dump (<code>dump name</code>), was showing that the <code>/word</code> field in that entry was indeed null instead of pointing to a red-word! value.</p>

<p>So, I then searched in <code>runtime/datatype.reds</code> where the <code>name-table</code> array is filled, for the code which is filling <code>/word</code> field. It appeared to be done by <code>make-words</code> function, which is called only once in <code>runtime/red.reds</code>. That's the runtime main wrapping code, loading all the datatypes. As event! is only loaded with View module, it seems logical that the <code>/word</code> field for event! entry cannot be filled by <code>make-words</code>, resulting in this null pointer crash.</p>

<p>For the second crash, this is how I proceeded:</p>

<p>I looked at <code>event?</code> function, and inserted a <code>??</code> to check the argument value:</p>

<pre><code>event?: routine [value [any-type!] return: [logic!]][?? value TYPE_OF(value) = TYPE_EVENT]
</code></pre>

<p>The value was <code>1</code> instead of a pointer to a <code>red-value!</code> structure, which is caused by the interpreter (<code>event?</code> is called by interpreter in the above case) forcing the conversion of integers from Red to R/S.</p>

<p>The tools I used the most for debugging Red's runtime code are: <code>probe</code>, <code>??</code>, <code>dump4</code>, <code>stack/dump</code> and <code>print-symbol</code>. Most of them are only available when code is compiled in debug mode. When it's a code generation issue (branching on wrong address, wrong opcodes emitted, etc..) I use <a href="http://www.heaventools.com/overview.htm">PE Explorer</a> for inspecting assembly code or <a href="https://www.hex-rays.com/products/ida/">IDA</a> when I need to step through the assembly code. On Linux/Mac, I use <code>objdump</code> for inspection and <code>gdb</code> for debugging at native level.</p>

<p>Hope this helps. ;-)</p>
