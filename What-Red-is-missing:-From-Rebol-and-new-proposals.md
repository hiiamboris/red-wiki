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

# Potential

abs             ; just an alias

# Rejected


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