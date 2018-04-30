# Goals

- List things that are missing from Red, which exist in Rebol.
- Propose additional mezzanines or alternate functionality.
- Set up preliminary rules to select the final set of mezzanines to include.
- Make 3 categories:
  - Accepted (consensus or needed in the Red runtime)
  - Potential (to be discussed and weightened)
  - Rejected (too narrow use-case(s), too heavy, etc...).

# Preliminary Rules

- If something is rejected, we should note why, so we don't keep asking to include it. It also gives people a chance to fix proposals.


# Thoughts

It might be nice to note (maybe we need tags we can easily list for various purposes) *why* something was accepted. e.g., is it a common need? Does it exist in other languages and will help lure people? Does having it prevent arguments of "Red doesn't even have X"? Is it a great example others can use to learn from? For example, how do you write control funcs correctly at the mezz level, and what do you need to consider in doing so? Copying an existing one as a starting point can help a lot. Does it show off Red's power and flexibility? *--Gregg*


----------------------------------------------------------------

# Accepted

    apply           ; But, PLEASE, a better way to propagate refinements
    array           ; (GI) I rarely use this myself
    build-markup    ; More general interpolation this is built on
    call
    collect-words
    decode-cgi      ; when the time comes
    delta-time      ; some form of profiling
    echo 
    forskip 
    map-each 
    now             ; NOW/TIME DONE
    remove-each     ; DONE
    repend          ; DONE
    sign?           ; DONE
    split           ; advanced version
    Usage 
    use             ; (GI) Condsider the value of this, name generality, and possible enhancements (e.g. spec block for 'words arg)
    get-env         ; DONE
    get-modes 
    list-env        ; DONE
    set-env         ; DONE
    set-modes
    open 
    close
    delete 
    delete-dir 
    exists?         ; DONE
    make-dir        ; DONE
    modified? 
    query 
    rename 
    to-red-file     ; DONE
    update 
    deline 
    enline 
    cp 
    dt
    strict-not-equal?
    bound?
    browse
    compress
    decompress 
    detab
    entab 
    free  
    hsv-to-rgb      ; (NR) in View module       
    input?          ; (NR) possible in a post 0.7.0 release
    protect         ; (NR) will wait until module system is designed
    unprotect       ; (NR) will wait until module system is designed
    recycle 
    rgb-to-hsv      ; (NR) in View module
    script?
    secure 
    trace           ; (NR) probably with different options and output format(s)
    closure 
    closure?
    more 
    to-<datatype>
    <datatype>?
    confirm
    editor 			; (NR) in GUI console
    span?			; (NR) in View module
    stylize 		; (NR) in View module

# Potential

    abs             ; just an alias
    assert
    build-tag       ; (NR) I would rather support it in `to-tag`.
    decode-url      ; (NR) I'm considering having such feature built in url! type.
    default
    dispatch        ; (NR) Not part of Rebol?
    for             ; (NR) A better alternative would be welcome. (GI) I will work on a proposal.
    found? 
    in-dir          ; (NR) Never ever used it...use-cases? (GI) Batch file processing and searching.
    join
    maximum-of      ; rename to at-max or find-max
    minimum-of      ; rename to at-min or find-min
    mod 
    reform 
    rejoin          ; DONE
    remold 
    throw-error 
    throw-on-error 
    info? 
    size? 
    dump-obj 
    maximum     alias for max 
    minimum     alias for min
    alias
    connected?      ; (NR) unsure this can be implemented in a reliable way
    create-link     ; (NR) needs better API, maybe incorporate this feature in file:// and dir:// ports
    decloak 
    do-browser
    encloak 
    last?           ; (NR) should be renamed to `single?` or something better...
    launch          ; (NR) API for running external executables might be redesigned (launch/run/call/...)
    run             ; (NR) API for running external executables might be redesigned (launch/run/call/...)
    native 
    read-io
    write-io
    unbind          ; (NR) will wait until module system is designed
    ajoin
    invalid-utf? 
    latin1?
    license  
    speed?
    true?
    utf?
    sixth 
    seventh 
    eighth 
    ninth 
    tenth
    parse-header 
    parse-header-date 
    parse-xml 
    protect-system
    get-net-info 
    read-cgi 
    read-net 
    set-net 
    to-idate        ; FORMAT will do this
    to-itime        ; FORMAT will do this
    to-relative-file 
    upgrade        ; (NR) feature will probably be moved to command-line option and console's menu.

    *-thru          ; (NR) very useful, though might need a nicer API?

    alert
    brightness?
    caret-to-offset
    offset-to-caret
    clear-fields
    confine
    edge-size?
    emailer
    find-window
    flash
    notify 
    get-style
    in-window?
    inform  
    make-face
    request 
    request-color 
    request-date 
    request-download 
    request-list 
    request-pass 
    request-text 
    screen-offset?
    set-font 
    set-para 
    set-style
    win-offset?

# Rejected

    include         ; (NR) removed from this list as it's premature before module system is designed
    access-os       ; (NR) needs a different user API, too Unix-specific
    path            ; undefined in Rebol
    dump-obj        ; (NR) feature already offered by `help`
    send            ; (NR) word reserved for another use, also irrelevant until (E)SMTP gets added
    build-attach-body ; (NR) irrelevant until (E)SMTP gets added, then probably supported with a different API

    dh-compute-key          ; (NR) Features rather supported through a crypt:// port or a better API
    dh-generate-key 
    dh-make-key 
    dsa-generate-key 
    dsa-make-key 
    dsa-make-signature 
    dsa-verify-signature 
    rsa-encrypt 
    rsa-generate-key 
    rsa-make-key

    bind?              ; (NR) useless alias to `bound?`
    crypt-strength?    ; (NR) irrelevant to Red
    disarm             ; (NR) irrelevant in Red (removed from R3 too)

    local-request-file ; (NR) irrevelant in Red, feature already offered by `request-file`

    reb-launch         ; undefined in Rebol
    textinfo           ; (NR) irrelevant in Red
    as-binary          ; (NR) irrelevant in Red
    as-string          ; (NR) irrelevant in Red
    component?         ; (NR) irrelevant in Red
    net-error          ; (NR) Rebol-specific
    open-events        ; (NR) Rebol-specific
    resolve            ; (NR) features available already using SET native
    title-of           ; (NR) Rebol-specific (no clue what this does...)

    ! 
    != 
    !== 
    ++ 
    -- 
    cvs-date           ; (NR) irrelevant in Red
    cvs-version        ; (NR) irrelevant in Red
    do-boot            ; (NR) Rebol-specific
    first+ 
    funct              ; (NR) available as `function`
    import-email 
    install            ; (NR) Rebol-specific
    link-app?          ; (NR) Rebol-specific
    link-relative-path ; (NR) Rebol-specific
    link?              ; (NR) Rebol-specific
    parse-email-addrs  ; (NR) Rebol-specific, should be handled by Red through email! datatype
    save-user          ; (NR) Rebol-specific
    set-user-name      ; (NR) Rebol-specific
    uninstall          ; (NR) Rebol-specific
    write-user         ; (NR) Rebol-specific

    choose             ; (NR) Rebol-specific
    clear-face         ; (NR) Rebol-specific
    deflag-face        ; (NR) Rebol-specific
    desktop            ; (NR) Rebol-specific
    do-face            ; (NR) Rebol-specific
    do-face-alt        ; (NR) Rebol-specific
    dump-pane          ; (NR) feature provided already with `dump-face`
    find-key-face      ; (NR) Rebol-specific
    flag-face          ; (NR) Rebol-specific
    flag-face?         ; (NR) Rebol-specific
    focus              ; (NR) Rebol-specific
    get-face           ; (NR) Rebol-specific
    hide               ; (NR) Rebol-specific
    hide-popup         ; (NR) Rebol-specific
    hilight-all        ; (NR) Rebol-specific
    hilight-text       ; (NR) Rebol-specific
    inside?            ; (NR) feature provided already with `within?`
    outside?           ; (NR) feature provided already with `not overlap?`
    reset-face         ; (NR) Rebol-specific
    resize-face        ; (NR) Rebol-specific
    scroll-drag        ; (NR) Rebol-specific
    scroll-face        ; (NR) Rebol-specific
    scroll-para        ; (NR) Rebol-specific
    set-face           ; (NR) Rebol-specific
    show-popup         ; (NR) Rebol-specific
    unfocus            ; (NR) Rebol-specific
    unlight-text       ; (NR) Rebol-specific
    viewed?            ; (NR) Rebol-specific
    viewtop            ; (NR) Rebol-specific

----------------------------------------------------------------

# Group A

    abs             ; just an alias
    apply           ; But, PLEASE, a better way to propagate refinements
    array           ; I rarely use this myself
    assert 
    build-markup    ; More general interpolation this is built on
    build-tag       ; Need tag! first
    call
    collect-words
    decode-cgi      ; when the time comes
    decode-url      ; when the time comes
    default 
    delta-time      ; some form of profiling
    dispatch        ; when the time comes
    echo 
    for 
    forskip 
    found? 
    in-dir 
    include 
    join
    map-each 
    maximum-of      ; rename to at-max or find-max
    minimum-of      ; rename to at-min or find-min
    mod 
    now 
    reform 
    rejoin 
    remold 
    remove-each 
    repend 
    sign? 
    split           ; advanced version
    throw-error 
    throw-on-error 
    Usage 
    use 

# OS/Env related

    access-os 
    get-env 
    get-modes 
    list-env 
    set-env 
    set-modes 

# I/O related

    open 
    close
    delete 
    delete-dir 
    exists? 
    info? 
    make-dir 
    modified? 
    query 
    rename 
    size? 
    to-rebol-file   ; name change
    update 


# Group B

    deline 
    enline 

    cp 
    dt
    dump-obj 
    maximum     alias for max 
    minimum     alias for min
    path 
    strict-not-equal? 

    send 
    build-attach-body 

    dh-compute-key 
    dh-generate-key 
    dh-make-key 
    dsa-generate-key 
    dsa-make-key 
    dsa-make-signature 
    dsa-verify-signature 
    rsa-encrypt 
    rsa-generate-key 
    rsa-make-key 

    alias 
    bind? 
    bound? 
    browse 
    compress 
    connected? 
    create-link 
    crypt-strength? 
    decloak 
    decompress 
    detab 
    disarm 
    do-browser 
    encloak 
    entab 
    free 
    hsv-to-rgb 
    input? 
    last? 
    launch 
    local-request-file 
    native 
    protect 
    read-io 
    reb-launch 
    recycle 
    rgb-to-hsv 
    run 
    script? 
    secure 
    textinfo 
    trace 
    unbind 
    unprotect 
    write-io 

# Group C

    ajoin 
    as-binary 
    as-string 
    closure 
    closure? 
    component? 
    invalid-utf? 
    latin1? 
    license 
    more 
    net-error 
    open-events 
    resolve 
    speed? 
    title-of 
    true? 
    utf? 

    sixth 
    seventh 
    eighth 
    ninth 
    tenth 

    parse-header 
    parse-header-date 
    parse-xml 
    protect-system 

    get-net-info 
    read-cgi 
    read-net 
    set-net 

    to-binary 
    to-bitset 
    to-block 
    to-char 
    to-closure 
    to-datatype 
    to-date 
    to-decimal 
    to-email 
    to-error 
    to-file 
    to-function 
    to-get-path 
    to-get-word 
    to-hash 
    to-idate        ; FORMAT will do this
    to-integer 
    to-issue 
    to-itime        ; FORMAT will do this
    to-library 
    to-list 
    to-lit-path 
    to-lit-word 
    to-logic 
    to-map 
    to-money 
    to-none 
    to-pair 
    to-paren 
    to-path 
    to-port 
    to-refinement 
    to-relative-file 
    to-set-path 
    to-set-word 
    to-string 
    to-tag 
    to-time 
    to-tuple 
    to-typeset 
    to-url 
    to-word 
    upgrade 

# Group D

    ! 
    != 
    !== 
    ++ 
    -- 
    cvs-date 
    cvs-version 
    do-boot 
    first+ 
    funct 
    import-email 
    install
    link-app? 
    link-relative-path 
    link? 
    parse-email-addrs 
    save-user 
    set-user-name 
    uninstall 
    write-user 

    do-thru 
    exists-thru? 
    launch-thru 
    load-thru 
    path-thru 
    read-thru 

# Type related

    any-type? 
    date? 
    decimal? 
    email? 
    event? 
    library? 
    list? 
    money? 
    port? 
    scalar? 
    struct? 
    tag? 
    time?
    
# GUI related

    alert 
    brightness? 
    choose 
    caret-to-offset 
    clear-face 
    clear-fields 
    confine 
    confirm 
    deflag-face 
    desktop 
    do-face 
    do-face-alt 
    dump-pane 
    edge-size? 
    editor 
    emailer 
    find-key-face 
    find-window 
    flag-face 
    flag-face? 
    flash 
    focus 
    get-face 
    get-style 
    hide 
    hide-popup 
    hilight-all 
    hilight-text 
    in-window? 
    inform 
    inside? 
    make-face 
    notify 
    offset-to-caret 
    outside? 
    request 
    request-color 
    request-date 
    request-download 
    request-list 
    request-pass 
    request-text 
    reset-face 
    resize-face 
    screen-offset? 
    scroll-drag 
    scroll-face 
    scroll-para 
    set-face 
    set-font 
    set-para 
    set-style 
    show-popup 
    span? 
    unfocus 
    stylize 
    unlight-text 
    viewed? 
    viewtop 
    win-offset? 