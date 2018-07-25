<table>
  <tr>
    <td>REP:</td>
    <td>XXXX</td>
  </tr>
  <tr>
    <td>Title:</td>
    <td>Adding other Red dialects</td>
  </tr>
  <tr>
    <td>Author(s):</td>
    <td>Donald Tsang</td>
  </tr>
  <tr>
    <td>Status:</td>
    <td>Draft</td>
  </tr>
  <tr>
    <td>Date Created:</td>
    <td>?????</td>
  </tr>
  <tr>
    <td>Date Last Actioned:</td>
    <td></td>
  </tr>
</table>

# Dialects of different programming paradigms
* Red/Script that acts like Python-Django, Ruby-Rails, PHP and Perl
    * A programming dialect that has the syntax structure of Python or Ruby
        * If one wants to write Ruby/Python/whatever, s/he better use Ruby/Python. This would provide no benefit for Red, and most people working on or waiting for Red do so precisely because they appreciate Red is not like Python or Ruby.
        * Since it is said that Red is the first "Full Stack Language", it would be good to at least give incentives to students who learnt Ruby/Python in school to learn Red once they finish their curriculum.
    * Having a Web-App library/framework that is similar to Rails/Django
        * In the sense of capable-enough solution, it will only take a "moderately finished" Red and some time before it will start happening (more accessible to general [coder] population)
        * But it will of course look different and I'd bet it will be more simple (less noise in the templates/code/routing etc.)
    * There has been an example of such a language, but it did not take off properly
        * https://github.com/hostilefork/rubol/
        * http://rubol.hostilefork.com/
* Red/Lisp that acts like Clojure/Haskell/Erlang/Scala
    * Please specify how exactly you think Clojure/Haskell/Erlang/Scala "act" i.e. what they or their approaches have in common and how would you extract it into dialect ?
    * also, Red is lispy-enough already (dialects/PARSE/different evaluation modes for arguments)
    * Is it possible to contain all lisp-like functions from these languages?
* Mobile Programming Languages that goes or compete with Haxe
* The Research Process
    * Firstly, seek input from relevant communities, and see what them a part of the category, 
    * Secondly, from the same community, see and what makes each language special or superior.
    * Thirdly, understand the characteristics of speciality and commonality of the language.
    * Fourthly, see what are the conditions that will make them switch programming languages.

https://en.wikipedia.org/wiki/Cross-platform#Cross-platform_programming_toolkits_and_environments

# Dialects of different internet languages
* Having Red/Web that works with or replaces HTML, CSS, XSLT, XML etc. (see W3C Recommendations)
    * Dialects for HTML ands CSS should be relatively simple . But are they necessary in (especially pre 1.0) core ? (in the future, I hope)
    * As soon as first web frameworks start to appear, we can be sure someone will write them (because HTML & CSS suck)
* Having Red/Database that works with or replaces JS, SQL, NoSQL-DB and other languages (see ISO standards) 
    * Red already is a nice dialect for Replacing JS (if someday JS gets added as compilation target).
    * Dialect for SQL generation seems TRIVIAL (write it as learning experience; doesn't IMO belong in core, rather into some SQL DB interfacing lib).
    * But what exactly is "NoSQL dialect" supposed to do (http://nosql-database.org/)
        * But a dialect is basically a notation, a way to describe something (other code can assign it some meaning, or transform it into another form). NoSQL is cover-term for a bunch old solutions for persisting data. [Hyper]Graph databases, key/value stores, document databases and [native] object store databases, there IMO cannot be "one dialect to talk to them them all". Start by bindings and dialects for querying concrete NoSQL stores, and where regularities will arise, you can abstract over them later.
    * Either Red is comprehensive enough to replace other languages, or is compatible enough to use with other languages.
        * No dialects needed. Red, Red/System. If it is necesary to interface leggacy code, there are (or will be) C ABI compatibility, Java/.Net/Cocoa bridges, sockets & protocols, what more is needed ?
            * I assume Red/System ⇒ Red (+ Red/Script) ⇒ Red/Web (+ Red/Database ?)
    * Library Integration (mostly JS)
        * What about SQL and DB Libraries?
    * https://en.wikipedia.org/wiki/Solution_stack
    * https://en.wikipedia.org/wiki/List_of_Apache–MySQL–PHP_packages
    * http://sixrevisions.com/web-technology/web-languages-decoded/

# Unification
* Allowance of Virtual Machines
    * Common Language Runtime (CLR)
    * Java virtual machine (JVM)
    * Low Level Virtual Machine (LLVM)
    * Mono, DotGNU or Portable.NET
    * Parrot virtual machine
* Intergration and unification
    * Text Chatroom protocol: IRC and XMPP
    * Email protocol: SMTP, IMAP and POP3
    * VoIP protocol: SIP/SIMPLE and Mumble
    * Secure protocol: TOX/DHT, Textsecure and Zephyr
    * Peer protocol: I2P, TOR, Freenet
    * IPFS?
* Unification of Lightweight/Humane/Simple document markup languages:
    * [AsciiDoc] (http://www.methods.co.nz/asciidoc/) [Creole] (http://www.wikicreole.org/) [JemDoc] (http://jemdoc.jaboc.net/)[Markdown] (http://daringfireball.net/projects/markdown/) [Markdown Extra] (https://michelf.ca/projects/php-markdown/extra/) [MediaWiki] (https://www.mediawiki.org/wiki/Help:Formatting) [Multimarkdown] (http://fletcherpenney.net/multimarkdown/) [Org-Mode] (http://orgmode.org/) [Pod] (https://www.mediawiki.org/wiki/MediaWiki) [PmWiki] (http://www.pmwiki.org/wiki/PmWiki/PmWiki) [RDoc] (http://docs.seattlerb.org/rdoc/) [Textile] (https://en.wikipedia.org/wiki/Textile_%28markup_language%29) [Texy] (http://texy.info/en/) [Txt2tags] (http://txt2tags.org/) [StructuredText] (http://docutils.sourceforge.net/rst.html)
    * AFT, deplate and KARAS


# Dialects for Easter Eggs (relatively small bits for giggles, only release for milestones)
* Allow a brainfuck-like script Red/Brain as an Easter Egg (1st milestone)
    * Classical 8-symbol BrainFuck commands (shift pointer, in/de-crement pointer, jumping, In/Out-put)
    * Includes Bitwise and Arithmatic Operators in bytes (add/subtract, multiply/divide, and/or/xor/imply/not)
    * Cutting, Copying, Pasting and Clearing data (like Emo and SPL)
    * Mutiple Tapes (like Nandypants and omnifuck) and  Multiple pointers (like Emo and Hargfak)
        * Cufrab and DoubleFuck contains both concepts
* Allow a golfing-specific script Red/Golf as an Easter Egg  (2nd milestone)
    * similar to Sclipting, Golfscript/Flogscript, CJam, APL/J/K, Pyth, Microscript, Owk, Retina, Fourier...
    * http://rebmu.hostilefork.com/ has done an example, but the Easter Egg would be more powerful than Rebmu.
    * Allows for data compression into base32, base64, base85 and/or base 91 to win byte-based competitions.
    * Short String Compression Functions like: [Shoco] (https://github.com/Ed-von-Schleck/shoco) [Skort] (https://github.com/jstepien/skrot) [smaz] (https://github.com/antirez/smaz)
* Allow a Cellular Automata or Microsoft-Logo-Esque (3rd milestone)
    * Langton's Ants and Multi-Colored Turmites
    * Conway's Game Of Life and Life-likes
    * Elementary Cellular Automata and Second-order Cellular Automata
    * Wireworld and Cyclic Cellular Automaton
    * (Von Neumann Cellular Automata or Nobili Cellular Automata)
    * (Codd's Cellular Automata or "Collect and Distribute"/"CoDi")
    * (Block Cellular Automata or Quantum Cellular Automata)
    * https://play.google.com/store/apps/details?id=be.evs.instinct&hl=en
* Other Machines (4th milestone)
    * https://en.wikipedia.org/wiki/Category:Educational_programming_languages
    * https://en.wikipedia.org/wiki/Category:Educational_abstract_machines
    * https://en.wikipedia.org/wiki/Category:Turing_machine

# References
* Office file formats
    * http://neowiki.neooffice.org/index.php/NeoOffice_File_Formats
    * https://wiki.openoffice.org/wiki/Documentation/OOo3_User_Guides/Getting_Started/File_formats
    * https://wiki.documentfoundation.org/Main_Page and https://en.wikipedia.org/wiki/LibreOffice#Supported_file_formats
* The last programming language reference (for an idea for what Red could shoot for): 
    * https://skillsmatter.com/skillscasts/2323-bobs-last-language 
    * http://lambda-the-ultimate.org/node/4312
* The Unicode 8.0.0 standards reference: 
    * ftp://unicode.org/Public/8.0.0/charts/CodeCharts.pdf
* Top Programming Languages According to RedMonk:

|            |       First Grade       |         Second Grade        |          Third Grade          |
|:----------:|:-----------------------:|:---------------------------:|:-----------------------------:|
|   Low-end  |          C, C++         |                             |            Rust, D            |
| Functional |          Scala          |   Clojure, Haskell, Erlang  |       F#, OCaml, Prolog       |
|  Scripting | PHP, Ruby, Python, Perl |                             |                               |
|     Web    |     Javascript, CSS     |      Coffeescript, XSLT     |         SQL, XML, TeX         |
|   Lispers  |                         |                             | EmacsLisp, CommonLisp, Scheme |
|     OO?    |      C#, ObjC, Java     | VB, Go, Swift, Arduino, Lua |                               |
|  Database? |      MatLab, Groovy     |                             |        Fortran, Delphi        |
|   Shell?   |          Shell          |          Powershell         |                               |
|   Others?  |                         |                             |     ASP, Dart, Tcl, Puppet, ColdFusion, Processing, Actionscript, Truescript   |


    * ASM languages:
        * https://en.wikibooks.org/wiki/X86_Assembly/x86_Assemblers
        * https://en.wikipedia.org/wiki/Comparison_of_assemblers
        * https://en.wikipedia.org/wiki/Microassembler/
        * http://c2.com/cgi/wiki?LearningAssemblyLanguage/
        * http://www.freebyte.com/programming/assembler/
        * http://www.cs.cornell.edu/talc/
        * http://asm.sourceforge.net/
        * http://www.int80h.org/
        * http://www.nasm.us/
        * http://www.plantation-productions.com/Webster/
* List of major distro families
    * Debian-based ditros
        * Debian
        * Ubuntu
        * Knoppix & sub-family
            * Knoppix
            * DamnSmall
            * Kanotix
            * Morphix
            * Kurumin
    * Slackware-based distros
        * Slackware
        * Gentoo
        * SuSE
        * Slax
    * Arch-based distros
        * Arch
    * Redhat-based distros
        * Redhat
        * Fedora
        * Mandriva
        * CentOS
* Other minor miscellaneous distros
        * Smoothwall
        * FREESCO
        * Sorcerer
        * Puppy
        * Ututo


* https://news.ycombinator.com/item?id=540048 
* http://neo4j.com/docs/stable/short-strings.html 
* http://www.pal-blog.de/entwicklung/perl/compressing-test-for-short-strings.html
* https://rubygems.org/gems/rsmaz/versions/0.0.4
* http://wiki.snappy-java.googlecode.com/hg/apidocs/org/xerial/snappy/Snappy.html
* http://journal.thobe.org/2011/02/better-support-for-short-strings-in.html
* http://www.gzip.org/algorithm.txt
* https://patentscope.wipo.int/search/en/detail.jsf?docId=WO2013176814
* http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0081414
* https://www.elastic.co/blog/store-compression-in-lucene-and-elasticsearch
* http://www.linuxtopia.org/online_books/programming_books/python_programming/python_ch33s08.html
* http://www.academia.edu/1360173/A_Short_Text_Compression_Scheme_based_on_Arithmetic_Coding