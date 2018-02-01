# Compile Red to Red/System

If we have a Red script named `test.red`:
```red
Red [ ]

; numbers
a: 1
b: 2 + a
print b

; strings
s: "hello"
prin s
print " world"

; function and call
inc: func [n][n + 1]
b: inc b
print b
```

We use `rebol` or `red-063.exe` to compile `test.red` and generate an executable:

```rebol
; red.r is from https://github.com/red/red/blob/master/red.r, you should download the 0.6.3 source code.
rebol>> do/args %red.r "--release %test.red"
```
or 
```red
red-063.exe --release -v 3 test.red > out.reds
```

We get the output `Red/System` code as `out.reds`:
```
with red [
    root-base: redbin/boot-load system/boot-data yes 
    exec: context [
        ------------| "Symbols" 
        ~datatype!: word/load "datatype!" 
        ~make: word/load "make" 
        ~unset!: word/load "unset!" 
        ~none!: word/load "none!" 
        ~logic!: word/load "logic!" 
        ~block!: word/load "block!" 
        ~string!: word/load "string!" 
        ~integer!: word/load "integer!" 
...省略...
        ~ctx348~fetch-options: word/load "ctx348~fetch-options" 
        ~ctx348~make-actor: word/load "ctx348~make-actor" 
        ~all?: word/load "all?" 

; 下面 3 个是 test.red 中声明的 int 变量和函数，但是没有字符串变量 s
        ~a435: word/load "a" 
        ~b436: word/load "b" 
        ~inc: word/load "inc" 
        ------------| "Literals" 
        ctx45: get-root-node2 89 
        ctx46: get-root-node2 92
...省略...
        ctx431: get-root-node2 1433 
        ctx437: get-root-node 2 
        ts438: as red-typeset! get-root 5 
        ------------| "Declarations" 
        ------------| "Functions" 
        f_inc: func [/local ctx saved ~n] [
            ctx: TO_CTX (ctx437) 
            saved: ctx/values 
            ctx/values: as node! stack/arguments 
            ~n: type-check ts438 0 stack/arguments 
            stack/mark-func-body words/_body 
            stack/reset 
            stack/mark-native ~+ 
            stack/push ~n 
            integer/push 1 
            actions/add* 
            stack/unwind 
            stack/reset ------------| 
            "n + 1" 
            stack/unwind-last 
            ctx/values: saved
        ] 
        ------------| "Main program" ; test.red 的主程序
        stack/mark-native ~set 
        word/push ~datatype! 
        datatype/push TYPE_DATATYPE 
        word/set 
        stack/unwind 
        stack/reset 
        #script %/Volumes/Red/samples/test/test.red 
        #either type = 'exe [stack/push get-cmdline-args] [none/push] 
        #script %/Volumes/Red/samples/test/test.red 
        either all [
            object/unchanged? ~system 203 
            object/unchanged2? ctx202 12 244
        ] [word/set-in-ctx ctx243 4] [
            stack/mark-func ~eval-set-path 
            if stack/arguments > stack/bottom [stack/push stack/arguments - 1] 
            set-path* eval-path _context/get ~system as cell! ~script as cell! ~args 
            stack/unwind
        ] 
        stack/reset ------------| {system/script/args: #system [ #either type = 'exe ...} 
        stack/mark-func ~extract-boot-args 
        f_extract-boot-args 
        stack/unwind ------------| "extract-boot-args" #user-code 
        stack/mark-native ~set 
        word/push ~a435 ----->变量 a
        integer/push 1 
        word/set 
        stack/unwind 
        stack/reset ------------| "a: 1" 
        stack/mark-native ~set 
        word/push ~b436 ----->变量 b
        stack/mark-native ~+ 
        integer/push 2 
        word/get ~a435 
        actions/add* 
        stack/unwind 
        word/set 
        stack/unwind 
        stack/reset ------------| "b: 2 + a" 
        stack/mark-native ~print 
        word/get ~b436 
        natives/print* true 
        stack/unwind 
        stack/reset ------------| "print b" 
        stack/mark-native ~set 
        word/push ~s -----------> 字符串变量 s ？
        string/push as red-string! get-root 0 
        word/set 
        stack/unwind 
        stack/reset ------------| {s: "hello"} 
        stack/mark-native ~prin 
        word/get ~s 
        natives/prin* true 
        stack/unwind 
        stack/reset ------------| "prin s" 
        stack/mark-native ~print 
        string/push as red-string! get-root 1 
        natives/print* true 
        stack/unwind 
        stack/reset ------------| {print " world"} 
        stack/mark-native ~set 
        word/push ~inc 
        _function/push get-root 3 get-root 4 ctx437 as integer! :f_inc null 
        word/set 
        stack/unwind 
        stack/reset ------------| "inc: func [n] [n + 1]" 
        stack/mark-native ~set 
        word/push ~b436 
        stack/mark-func ~inc 
        word/get ~b436 
        f_inc 
        stack/unwind 
        word/set 
        stack/unwind 
        stack/reset ------------| "b: inc b" 
        stack/mark-native ~print 
        word/get ~b436 
        natives/print* true 
        stack/unwind ------------| "print b" #user-code
        ]
    ]
]
...compilation time : 47 ms
```

# What's going on about the compile process?

- How did the `test.red` compiled to `out.reds`? 
- How did the Red runtime built in to the final executable `test.exe`? 
- Where is the Red runtime starts?

I just make a sequence diagram for this compile process, using [PlantUML](http://plantuml.com/sequence-diagram), in order to see how Red code compiled to Red/System and where Red runtime starts.

From the compiling process, I found the `test.red` compiled to standard `Red/System`:
```
Red/System [origin: 'Red] 

red: context [...] ; The Red runtime context

red/init ; Initial Red runtime (allocate memory and other things).

with red [
    exec: context [
        ------------| "Symbols" 
        ------------| "Literals"
        ------------| "Declarations"
        ------------| "Functions"
        ------------| "Main program"
    ]
]
```
Now I know the Red runtime is stars from [runtime/red.reds](https://github.com/red/red/blob/master/runtime/red.reds#L139), and it's memory model is here [runtime/allocator.reds](https://github.com/red/red/blob/master/runtime/allocator.reds#L84).

Go Red!

[Here](http://www.plantuml.com/plantuml/uml/tLXjSzku4Vui_egDCkxPcb2Kx7CSwowAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86Tzw2A_CaydJnxFK8dwkhULEkfBgG71h9r91MdXh57RUfMULLL5A-AB2PVQ5JlaYAmUGQnYvSqxp8g7AMRzYY37UhuBX5mMRkH05cZyFuP0gAQfZT5ZnLS4-acjl4VfPfJ1ysGcfb4jChQm8ZELm8eU4lW8s12QxmZHFDxgbvmvQTtWs7mwEQFY_xgf7qFnBGnnZGhZYPS3Q5fisIZi1OyRpyDa5hme9XhhQOe2o7y9EY-MtayOQJbR0Nrhx-HMwxTj9ATddAMdsiMBzed7fHhXsmXgtbh0TtTAZGldztPJCSPYI8ZVEjhmgr9A_9CTg1q2lZcXME32hYGkEaWyD7I1dr-eD1JS8US33eHprlepO7UKjxn_Jm518ZKBKJkqZsD2A0eKv8FEbrWnp53h71t7MpdR2QUUfRFCMan8Gq60nx60WNK2riALcIOZNdeGA69N31gMqaneuDupVSFGmS7RQnqBiV_NXgRAy518DFi-2xKl7kREHSdAmnoRxnYtUdnrDWjxYcYbnNFGIKJ4sKO0-zE3i_ioISOaUCosJ-OBTXjYXOdbMhTd7UsMbEn--cbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cvsDdpvWeBBRMfXtfOyQHmU1xGFozr810RPh58eqlcvmW6tCNOUY3QnlIWq4KKCw4P4lC1FW95-TWT4tC10M_30OiA0c-NI5uYnSLenJC3CsYqF3ll_rs_HimXHbCuRkqwMqZy0EI6vLHpzsO3pExPfuuU9wWV_Eunfa7XwakWiyCEoj1YYw2JpJDyV9YYrOdqHLsl-SfViUODpUv9Tw37D4yck-oyKUJiTiX2_HijDVTZ9_I856zpT_0akBjrWAjAGjD5aXIpznQzBypmy9UjDlznDTqr-AoAe5zkZM3lSRsv7pqTkf2rUiMevLoiJGr1JzO-WeyMvo4OURV_nIBE2EQ92Vp7s3lKdm26xopm-oW7DBXO3XCcfFx72XFoUP3PxklipIQ-Ct4r-54R7bPLTn7qEpj7Q3iRdZBVVGzgZR0sN8NET80KuvApyDgQOwuw7M9EyZehKr0z9KzQx2r9gaKUwicjcw5N2TRna-Ecxlr2qie1psZ7pTK9KhB8iN8fQgRfdpROv-sQkf7mkQocm_hYpWkrXkUiMVB_1GQqTHdLJciOtAgzyHd0h1yDM7sOw7o0hCozq8tLpJsIe7NEX_IvdSKmPrD_8uFSl-rkkUf2AlKrFx3gSqFJriXC_N611fsCRnNmfPX1dfPR-n9tjYx2IneBhRDslH2ImYADxffRWh_Fv39biAAtDEzkezbl-mZGRluVsm5jGW8Ea1xnNOkn8MoRYEYlFfFTesy5H5sYtyf4qaOfCRmRTIcDzH_Kx0rwJb8hOBPOW5kzB9UhDRYW9qlAVNxx9gkXNoi5pRtiuZRmJdOUWR775jkNi6iPvvVuElb9rCsiJRmb1csFfXzKO8DClld3QvAIhAefuKcUGzc34DfIUMhUS_nYMEmMruJKEo_j58rWqFqv-9HKtKKzNYyGbR8WsCAnBVeOkhP62vc3mJEiwPtsqtp2vJI1cVA7GP-Ez1jvezGA9e5S0FKXELT0irUkq1LAegEPrI5o51Jjzk4bhgsO-lAXXmCrglR3YtrRHThVxPlzu_TSex1XeMjS5lZqUiTZeyi2wHevv0MKbB-70_FOaqUsZdWHZTQUHKMfTqNRDaP7xXNE_iyYapEn1X_vjA7oxEevu8niP4vWTweMl66ci0VuzylWrNK3-d-EAuj6eLnjGwjrQCBV2tM-Vy0) is the PlantUML source to generate sequence diagram, it must exists many mistakes. Hope you could correct my mistakes, thanks!

You can change the source below and copy to [PlantUML online server](http://www.plantuml.com/plantuml/uml) to regenerate a new diagram.


# sequence diagram([click here to see origin SVG image](http://www.plantuml.com/plantuml/svg/tLXjSzku4Vui_egDCkxPcb2Kx7Fiw2wAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86zwu5L-P9vEdYsUiHFbTNywPSINKXEJIIgI6jF3MAEsvJiyghgAHyKM0p-qAdV94KWyarZ5sufdcHKUKitx156UvMmN6BWitSYGBC7eVnoHGKrJ2xBNYgu9v8DRU9_IpJc3viXDJA9APNrWH6ShWGGiDV01i34rpX6oUQtLFpXYqxlHiEXqSrVLxsLIFfVoIWZp6WNN4ouMmAJPib7O6nuhlosmIj2Gk6kTfYWh8Sma-AvhUHnnjELi9UMFlw5xdjsaahsUKfQ_MnOlsYSUf5kNR06hQMinxSqQ52-_xTbSrmc94WDiwtlYhKahmcnsW7Gg-CQrOuCgY82usJ3WuV8MNKwmy6DGbxmC6W7_MyZTeSv2tj7TF3KqYCGDLEx2BPquW0XZeXyQJN3N8KEyG6SzVDTi9gvgblyHIJ4XFIO33iOI1SGBMmfMH9YjMSXmeObi05fxQH63isZDzmzp1mTDZ6Gkz-zEEfiRqM40m-pOBlIiNvsSYvE5bZadtZ5-UdnrDWjxYcYbnNFGIKJ4sKO0-zE7jtPaaunOuObydymMx3R56mFAjMxUAyizESZi-dbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cxyREdZ5HM6ojJJlInumZWy7tWFXxgG61sZIBHHfVDZb1DkOkmj07rpQa1uCee9m9oPQO2V0JBix1w9gO2Gf-6GnOK19ykqBm5YygHYcO6Pf5eU7VV_lj-ZTW2ZEOmdTfqzj6u0SaDogZdhin7sPspJnny3n1_-PnZJCF3bDT19yPTbQ255q4dsYQu-N55grEeYliVizJ_8unRsvoIxm7EQ9vDDzbuuycOxT35kZPQA-x6J-bGQ9wcx-19SNRhGLQKnQQB92adhcrw3ytqynUjDlznDTqr-AoAe5zkZM3lSRsvFmCXn0RVLwt6AsKYwMfAVZ1qbVWsU8a33F__gTOn1dH93cPFy_-RFnswbDy0XkyiyFie1pIuM0uJ9gJ-nmeJydcGsUxHvPdEvQeYDzBV1bEn9QNHSLz1BtUNW7AveQNt4VVechpcwrsaICDCEMaz3oibkM8YrwNk8ECtjhyGrVPemPRQv9AiRTgGkjMo7MoPVpakBbRlw-4Sj0n-d53LQAqB58CNAgwQSgtFVPfgwP-8cmkk_6mleZJOhtV6NksZ4-X4KrvLfN3EWQdUajy8VxEfjLeOeBjnoBcMPwdvyS-PNmYXawlIEtz0kzuFhxDZog2Emlv7H_n_cjrwLCHbgof_OSZd9-aja9cweq9EsnfUCc4BSDqzBBUs0EziHSLMD1SR9k5weN-45HoTDhST85_89CjXHMvftjD07j0sLI2tl7_s1TgWH2qW_RoxLo82sKHHqJvEy1arzLR4M7h-9SoaOInQWpVHcqRHVqN1bkYdiFK2fgj7U06CUl6QYqEsFaAKx_ThkbQoCTwdNC_ahWTd8gjQt3eiTNd5Snwvla5lbv-CbKKImLFcM7hezdNADWWit_UuAga98jwMcYIzs36C9QMLBwQznoVE0QxvJK1nVvBArKpE4I7BXurLKTHZyShP8KyEEP0V8i-ffwDuMBsGE1qQpgvtJI_HobjSARwOUIt0zrc_WY5eba0Dq1DMDubqEor2rIbekXvHbM833LrkqregMixjgvioC5WlRhjq5BVTh7sH_ns_zWjvnZiNzWPjpKUlTxX-CYoG8fs3caX9UNFyV0irkIXdGDkTAELLs5Hsdt1bftnWNkriy-ZoEHCZF5dhe2uF8jwBXWJ6vaJw8sg5M-e2Vmn_lqoM4NzaEwFuzgYKHXNwz1QGRh0rsAT_my0))

![sequence diagram](http://www.plantuml.com/plantuml/svg/tLXjSzku4Vui_egDCkxPcb2Kx7Fiw2wAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86zwu5L-P9vEdYsUiHFbTNywPSINKXEJIIgI6jF3MAEsvJiyghgAHyKM0p-qAdV94KWyarZ5sufdcHKUKitx156UvMmN6BWitSYGBC7eVnoHGKrJ2xBNYgu9v8DRU9_IpJc3viXDJA9APNrWH6ShWGGiDV01i34rpX6oUQtLFpXYqxlHiEXqSrVLxsLIFfVoIWZp6WNN4ouMmAJPib7O6nuhlosmIj2Gk6kTfYWh8Sma-AvhUHnnjELi9UMFlw5xdjsaahsUKfQ_MnOlsYSUf5kNR06hQMinxSqQ52-_xTbSrmc94WDiwtlYhKahmcnsW7Gg-CQrOuCgY82usJ3WuV8MNKwmy6DGbxmC6W7_MyZTeSv2tj7TF3KqYCGDLEx2BPquW0XZeXyQJN3N8KEyG6SzVDTi9gvgblyHIJ4XFIO33iOI1SGBMmfMH9YjMSXmeObi05fxQH63isZDzmzp1mTDZ6Gkz-zEEfiRqM40m-pOBlIiNvsSYvE5bZadtZ5-UdnrDWjxYcYbnNFGIKJ4sKO0-zE7jtPaaunOuObydymMx3R56mFAjMxUAyizESZi-dbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cxyREdZ5HM6ojJJlInumZWy7tWFXxgG61sZIBHHfVDZb1DkOkmj07rpQa1uCee9m9oPQO2V0JBix1w9gO2Gf-6GnOK19ykqBm5YygHYcO6Pf5eU7VV_lj-ZTW2ZEOmdTfqzj6u0SaDogZdhin7sPspJnny3n1_-PnZJCF3bDT19yPTbQ255q4dsYQu-N55grEeYliVizJ_8unRsvoIxm7EQ9vDDzbuuycOxT35kZPQA-x6J-bGQ9wcx-19SNRhGLQKnQQB92adhcrw3ytqynUjDlznDTqr-AoAe5zkZM3lSRsvFmCXn0RVLwt6AsKYwMfAVZ1qbVWsU8a33F__gTOn1dH93cPFy_-RFnswbDy0XkyiyFie1pIuM0uJ9gJ-nmeJydcGsUxHvPdEvQeYDzBV1bEn9QNHSLz1BtUNW7AveQNt4VVechpcwrsaICDCEMaz3oibkM8YrwNk8ECtjhyGrVPemPRQv9AiRTgGkjMo7MoPVpakBbRlw-4Sj0n-d53LQAqB58CNAgwQSgtFVPfgwP-8cmkk_6mleZJOhtV6NksZ4-X4KrvLfN3EWQdUajy8VxEfjLeOeBjnoBcMPwdvyS-PNmYXawlIEtz0kzuFhxDZog2Emlv7H_n_cjrwLCHbgof_OSZd9-aja9cweq9EsnfUCc4BSDqzBBUs0EziHSLMD1SR9k5weN-45HoTDhST85_89CjXHMvftjD07j0sLI2tl7_s1TgWH2qW_RoxLo82sKHHqJvEy1arzLR4M7h-9SoaOInQWpVHcqRHVqN1bkYdiFK2fgj7U06CUl6QYqEsFaAKx_ThkbQoCTwdNC_ahWTd8gjQt3eiTNd5Snwvla5lbv-CbKKImLFcM7hezdNADWWit_UuAga98jwMcYIzs36C9QMLBwQznoVE0QxvJK1nVvBArKpE4I7BXurLKTHZyShP8KyEEP0V8i-ffwDuMBsGE1qQpgvtJI_HobjSARwOUIt0zrc_WY5eba0Dq1DMDubqEor2rIbekXvHbM833LrkqregMixjgvioC5WlRhjq5BVTh7sH_ns_zWjvnZiNzWPjpKUlTxX-CYoG8fs3caX9UNFyV0irkIXdGDkTAELLs5Hsdt1bftnWNkriy-ZoEHCZF5dhe2uF8jwBXWJ6vaJw8sg5M-e2Vmn_lqoM4NzaEwFuzgYKHXNwz1QGRh0rsAT_my0)


# PlantUML source code

```shell
@startuml
skinparam titleBorderRoundCorner 15
skinparam titleBorderThickness 2
skinparam titleBorderColor red
skinparam titleBackgroundColor Aqua-CadetBlue
title Red compile process and runtime initial\n\nBase on Red 0.6.3(https://github.com/red/red/releases)\n\nSee: http://www.red-lang.org/2011/05/redsystem-compiler-overview.html

skinparam ParticipantPadding 20
skinparam BoxPadding 10

actor Reducer

box "Red command-line front-end" #LightBlue
    participant "red.r\n\nredc: context" as red.r
end box

box "Red compiler"
    participant "compiler.r\n\nred: context" as redcompiler 
end box

box "Red/System compiler" #DarkSalmon
    participant "system/compiler.r\n\nsystem-dialect: context" as rscompiler    
    participant "system/linker.r\n\nlinker: context" as linker
    participant "system/emitter.r\n\nemitter: context" as emitter
    participant "system/loader.r\n\nloader: context" as loader
    participant "system/lexer.r\n\nlexer: context" as lexer
    participant "system/utils/libRedRT.r\n\nlibRedRT: context" as libRedRT.r
end box

box "Red runtime initial" #FFBBBB
    participant "runtime/red.reds\n\nred: context" as redrt
    participant "runtime/allocator.reds" as redalloc
end box

autonumber "<font color=red><b>Step-0  "
Reducer -> red.r : rebol>> do/args %red.r "--release %tests/hello.red"

||45||
== Load compiler toolchain(1): compiler / linker / emitter ==

red.r -> redcompiler : @17> do-cache %compiler.r
redcompiler -> rscompiler : @10> do-cache %system/compiler.r
rscompiler -> linker : @19> do-cache %system/linker.r
linker -[#0000FF]-> rscompiler : return linker: context
rscompiler -> emitter : @20> do-cache %system/emitter.r
emitter -[#0000FF]-> rscompiler : return emitter: context

||45||
== Prepare Red runtime includes ==

rscompiler -> libRedRT.r : @21> do-cache %system/utils/libRedRT.r
libRedRT.r -> libRedRT.r : @13> set [funcs vars] load-cache %system/utils/libRedRT-exports.r\ninclude red/(boot & actions & natives & stack...)
libRedRT.r -[#0000FF]-> rscompiler : return libRedRT: context

||45||
== Load compiler toolchain(2): loader / lexer ==

rscompiler -> loader : @29> loader: do bind load-cache %system/loader.r 'self
loader -> lexer : @10> do-cache %lexer.r
lexer -[#0000FF]-> loader : return lexer: context
loader -[#0000FF]-> rscompiler : return loader: context
rscompiler -[#0000FF]-> redcompiler : return system-dialect: context

||45||
== Initial Red compiler options ==

redcompiler -> redcompiler : load other things
note over redcompiler
	"**Red compiler load other things**"
	lexer: do bind load-cache %lexer.r 'self
	extracts: do bind load-cache %utils/extractor.r 'self
	redbin:	do bind load-cache %utils/redbin.r 'self
	preprocessor: do-cache file: %utils/preprocessor.r
	preprocessor: do preprocessor/expand/clean load-cache file none
end note

redcompiler -[#0000FF]-> red.r : return red: context

||45||
== Compile Red to Red/System ==

red.r -> red.r : @870> redc/main\n@852> if result: compile src opts\n@797> if needs-libRedRT? opts [build-libRedRT opts]
red.r -> redcompiler : @518> result: red/compile script opts
redcompiler -> rscompiler : @4673> if file? file [system-dialect/collect-resources src/1 resources file]
rscompiler -> redcompiler : return %system/assets/red.ico image
redcompiler -> redcompiler : @4691: comp-as-exe src
note over redcompiler
    **`comp-as-exe` return a Red/System code template and its Redbin**
Red/System [origin: 'Red] 
red/init ;**initial Red runtime**
with red [ ;**but WHERE is the `red context?**
    exec: context [
        ------------| "Symbols" 
        ------------| "Literals"
        ------------| "Declarations"
        ------------| "Functions"
        ------------| "Main program"
    ]
]
end note
redcompiler -[#0000FF]-> red.r : return Red/System code template above


||45||
== Load and compile Red runtime, generate a executable ==

red.r -> rscompiler : @820> system-dialect/compile/options/loaded src opts result
rscompiler -> rscompiler : @3871> comp-runtime-prolog to logic! loaded all [loaded job-data/3]
rscompiler -> rscompiler : @3716> script: pick [%red.reds %../runtime/red.reds] encap?
rscompiler -> loader : @3717> script: job loader/process/own script script
loader -> redrt : load many things Red runtime needs
redrt -> redrt : include many things Red runtime needs
note over redrt 
#include %definitions.reds
#include %macros.reds
#include %platform/win32.reds
#include %allocator.reds
#include %datatypes/structures.reds
#include %datatypes/datatype.reds
#include %actions.reds
#include %natives.reds
#include %stack.reds
#include ...
end note

redrt --> loader : return Red runtime files
loader --> rscompiler : return red: context

rscompiler -> rscompiler : @3717> compiler/run job loader/process/own script script
rscompiler -> rscompiler : @3582> comp-dialect
note over rscompiler
emit %runtime/common.reds
emit %red.reds
emit %hello.reds
--->
---> In other words, this step will compile, load, emit below Red/System code
Red/System [origin: 'Red] 
red: context [...] ;**Now we have the `red context**
red/init
with red [
    exec: context [
        ------------| "Symbols" 
        ------------| "Literals"
        ------------| "Declarations"
        ------------| "Functions"
        ------------| "Main program"
    ]
]
end note

rscompiler --> red.r : generate a executable hello.exe
red.r --> Reducer : return a executable hello.exe


||45||
== User run the executable hello.exe ==

Reducer -> redrt : run the executable hello.exe
redrt -> redalloc : red/init
redalloc --> redrt : allocate Red runtime memory
note over redalloc
memory: declare struct! [					; TBD: instanciate this structure per OS thread
	total	 [integer!]						;-- total memory size allocated (in bytes)
	n-head	 [node-frame!]					;-- head of node frames list
	n-active [node-frame!]					;-- actively used node frame
	n-tail	 [node-frame!]					;-- tail of node frames list
	s-head	 [series-frame!]				;-- head of series frames list
	s-active [series-frame!]				;-- actively used series frame
	s-tail	 [series-frame!]				;-- tail of series frames list
	s-start	 [integer!]						;-- start size for new series frame		(1)
	s-size	 [integer!]						;-- current size for new series frame	(1)
	s-max	 [integer!]						;-- max size for new series frames		(1)
	b-head	 [big-frame!]					;-- head of big frames list
]

init-mem: does [
	memory/total: 	0
	memory/s-start: _1MB
	memory/s-max: 	_2MB
	memory/s-size: 	memory/s-start
]
end note

@enduml
```