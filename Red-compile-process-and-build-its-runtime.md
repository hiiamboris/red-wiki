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
; red.r is from https://github.com/red/red/blob/master/red.r,
; you should download the 0.6.3 source code.

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

[Here](http://www.plantuml.com/plantuml/uml/tLXjSzku4Vui_egDCkxPcb2Kx7Fiw2wAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86zwu5L-P9vEdYsUiHFbTNywPSINKXEJIIgI6jF3MAEsvJiyghgAHyKM0p-qAdV94KWyarZ5sufdcHKUKitx156UvMmN6BWitSYGBC7eVnoHGKrJ2xBNYgu9v8DRU9_IpJc3viXDJA9APNrWH6ShWGGiDV01i34rpX6oUQtLFpXYqxlHiEXqSrVLxsLIFfVoIWZp6WNN4ouMmAJPib7O6nuhlosmIj2Gk6kTfYWh8Sma-AvhUHnnjELi9UMFlw5xdjsaahsUKfQ_MnOlsYSUf5kNR06hQMinxSqQ52-_xTbSrmc94WDiwtlYhKahmcnsW7Gg-CQrOuCgY82usJ3WuV8MNKwmy6DGbxmC6W7_MyZTeSv2tj7TF3KqYCGDLEx2BPquW0XZeXyQJN3N8KEyG6SzVDTi9gvgblyHIJ4XFIO33iOI1SGBMmfMH9YjMSXmeObi05fxQH63isZDzmzp1mTDZ6Gkz-zEEfiRqM40m-pOBlIiNvsSYvE5bZadtZ5-UdnrDWjxYcYbnNFGIKJ4sKO0-zE7jtPaaunOuObydymMx3R56mFAjMxUAyizESZi-dbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cxyREdZ5HM6ojJJlInumZWy7tWFXxgG61sZIBHHfVDZb1DkOkmj07rpQa1uCee9m9oPQO2V0JBix1w9gO2Gf-6GnOK19ykqBm5YygHYcO6Pf5eU7VV_lj-ZTW2ZEOmdTfqzj6u0SaDogZdhin7sPspJnny3n1_-PnZJCF3bDT19yPTbQ255q4dsYQu-N55grEeYliVizJ_8unRsvoIxm7EQ9vDDzbuuycOxT35kZPQA-x6J-bGQ9wcx-19SNRhGLQKnQQB92adhcrw3ytqynUjDlznDTqr-AoAe5zkZM3lSRsvFmCXn0RVLwt6AsKYwMfAVZ1qbVWsU8a33F__gTOn1dH93cPFy_-RFnswbDy0XkyiyFie1pIuM0uJ9gJ-nmeJydcGsUxHvPdEvQeYDzBV1bEn9QNHSLz1BtUNW7AveQNt4VVechpcwrsaICDCEMaz3oibkM8YrwNk8ECtjhyGrVPemPRQv9AiRTgGkjMo7MoPVpakBbRlw-4Sj0n-d53LQAqB58CNAgwQSgtFVPfgwP-8cmkk_6mleZJOhtV6NksZ4-X4KrvLfN3EWQdUajy8VxEfjLeOeBjnoBcMPwdvyS-PNmYXawlIEtz0kzuFhxDZog2Emlv7H_n_cjrwLCHbgof_OSZd9-aja9cweq9EsnfUCc4BSDqzBBUs0EziHSLMD1SR9k5weN-45HoTDhST85_89CjXHMvftjD07j0sLI2tl7_s1TgWH2qW_RoxLo82sKHHqJvEy1arzLR4M7h-9SoaOInQWpVHcqRHVqN1bkYdiFK2fgj7U06CUl6QYqEsFaAKx_ThkbQoCTwdNC_ahWTd8gjQt3eiTNd5Snwvla5lbv-CbKKImLFcM7hezdNADWWit_UuAga98jwMcYIzs36C9QMLBwQznoVE0QxvJK1nVvBArKpE4I7BXurLKTHZyShP8KyEEP0V8i-ffwDuMBsGE1qQpgvtJI_HobjSARwOUIt0zrc_WY5eba0Dq1DMDubqEor2rIbekXvHbM833LrkqregMixjgvioC5WlRhjq5BVTh7sH_ns_zWjvnZiNzWPjpKUlTxX-CYoG8fs3caX9UNFyV0irkIXdGDkTAELLs5Hsdt1bftnWNkriy-ZoEHCZF5dhe2uF8jwBXWJ6vaJw8sg5M-e2Vmn_lqoM4NzaEwFuzgYKHXNwz1QGRh0rsAT_my0) is the PlantUML source to generate sequence diagram, it must exists many mistakes. Hope you could correct my mistakes, thanks!

You can change the source below and copy to [PlantUML online server](http://www.plantuml.com/plantuml/uml) to regenerate a new diagram.


# sequence diagram([click here to see origin SVG image](https://camo.githubusercontent.com/1ee300546ffc657621b19042e43feda7eb58d576/687474703a2f2f7777772e706c616e74756d6c2e636f6d2f706c616e74756d6c2f7376672f744c586a537a6b75345675695f656744436b78506362324b7837466977327741772d4779784b6f6b70545a667a3845596b4b306142453543304965304d6452374678774235723857485468764e43784f6565317a54585678336271654a51476b7264636178585a563434644d6539644577514d4736504e4e656b3350486f3476624a302d525156766b634266375154416d4e34786d4b5548326d634950646c52394272524959563261467072466d4d394676414377696b7965393662574d6b514749684d3675526c366f624938755a6d33364a31444c6a4a4f396e66486c396c5f316b5f3938673275314f6f3668755464586f6b6a44776549503869634c754c596f376f494c30465f76544a6635543731646233774747434354396b6a7a69583469477675536b5861436c61553347553977444a57724146496a447278464d486952596479667848784e3262714f4a312d5277574c4c644144654a68426f4a4236355f3279495a4f6c6e47467656667535344b61724e582d72426a38674f6f59584e59306c5a5f753657795276756e4a6b394d3277765a6f68302d6c467846626f626139794464696634345672486e41536f58794949563857636c7765466a303544596a323361304967594269555666787443686a6f674d556e6e42326a586464446e4f552d74756d2d6a464844785461376d6a2d427569507a7361426a4152463443567172477466416a414533775433443577546f4b787a727664757a4f77754e4a446a32786e5f616b54574c5f694c6131574b395341734655363064516a3671755641684876524f33444b59557371326e4e513834356b6b4677517361337a77726658504163394b3851734f51456c4267776e4153625f316e624f63434443774f6852414a6b6e663879356f643162423053496548547455623632597275694c7757795f6b5f745939706238314c763372494a647a544a3679717453475a57377861716d6859674b693063453364476b4a4a414d4759384e41667543316a7a45464f626d4b757135486662516d65325a4c517a4146657755646471775564774672782d38702d676538516a3131766b5941433775774639686c626e43534c6c6c5a755746566c657958395368664b6a314575363979505455414b66326b4b6c696b314141477043354c335a4b424b4e573935304a4c596c4b38367a7775354c2d503976456459735569484662544e79775053494e4b58454a49496749366a46334d414573764a69796768674148794b4d30702d7141645639344b577961725a357375666463484b554b69747831353655764d6d4e364257697453594742433765566e6f48474b724a3278424e596775397638445255395f49704a6333766958444a413941504e7257483653685747476944563031693334727058366f5551744c4670585971786c48694558715372564c78734c494666566f49575a7036574e4e346f754d6d414a506962374f366e75686c6f736d496a32476b366b546659576838536d612d41766855486e6e6a454c6939554d466c773578646a7361616873554b66515f4d6e4f6c7359535566356b4e52303668514d696e7853715135322d5f78546253726d63393457446977746c59684b61686d636e73573747672d4351724f75436759383275734a33577556384d4e4b776d7936444762786d433657375f4d795a5465537632746a375446334b71594347444c457832425071755730585a655879514a4e334e384b45794736537a5644546939677667626c7948494a345846494f3333694f49315347424d6d664d4839596a4d53586d654f6269303566785148363369735a447a6d7a70316d54445a36476b7a2d7a4545666952714d34306d2d704f426c49694e76735359764535625a6164745a352d55646e7244576a78596359626e4e4647494b4a34734b4f302d7a45376a74506161756e4f754f627964796d4d78335235366d46416a4d78554179697a45535a692d646249584a5232666e674d47525254374654767356747677544d367343444a797563797867514f3143535f4539334c4d394767394e7036634943556f2d367a59794b4b6a5134776f693671485857524e314373496630424f63495f66797667326378795245645a35484d366f6a4a4a6c496e756d5a577937745746587867473631735a494248486656445a6231446b4f6b6d6a3037727051613175436565396d396f50514f3256304a426978317739674f3247662d36476e4f4b3139796b71426d355979674859634f3650663565553756565f6c6a2d5a5457325a454f6d645466717a6a3675305361446f675a6468696e37735073704a6e6e79336e315f2d506e5a4a43463362445431397950546251323535713464735951752d4e353567724565596c6956697a4a5f38756e5273766f49786d374551397644447a62757579634f785433356b5a5051412d78364a2d624751397763782d3139534e5268474c514b6e515142393261646863727733797471796e556a446c7a6e445471722d416f4165357a6b5a4d336c53527376466d43586e3052564c77743641734b59774d6641565a3171625657735538613333465f5f67544f6e3164483933635046795f2d52466e737762447930586b796979466965317049754d30754a39674a2d6e6d654a79646347735578487650644576516559447a42563162456e39514e48534c7a314274554e5737417665514e7434565665636870637772736149434443454d617a336f69626b4d385972774e6b384543746a687947725650656d50525176394169525467476b6a4d6f374d6f505670616b4262526c772d34536a306e2d6435334c514171423538434e41677751536774465650666777502d38636d6b6b5f366d6c655a4a4f687456364e6b735a342d58344b72764c664e334557516455616a7938567845666a4c654f65426a6e6f42634d5077647679532d504e6d595861776c4945747a306b7a75466878445a6f6732456d6c7637485f6e5f636a72774c434862676f665f4f535a64392d616a6139637765713945736e6655436334425344717a4242557330457a6948534c4d44315352396b3577654e2d3435486f544468535438355f3839436a58484d7666746a4430376a30734c4932746c375f7331546757483271575f526f784c6f3832734b4848714a7645793161727a4c52344d37682d39536f614f496e51577056486371524856714e31626b596469464b3266676a3755303643556c3651597145734661414b785f54686b62516f435477644e435f616857546438676a5174336569544e6435536e77766c61356c62762d43624b4b496d4c46634d3768657a644e4144575769745f557541676139386a774d6359497a7333364339514d4c4277517a6e6f5645305178764a4b316e56764241724b7045344937425875724c4b54485a79536850384b79454550305638692d66667744754d42734745317151706776744a495f486f626a534152774f554974307a72635f57593565626130447131444d44756271456f7232724962656b587648624d3833334c726b717265674d69786a6776696f4335576c52686a7135425654683773485f6e735f7a576a766e5a694e7a57506a704b556c5478582d43596f473866733363615839554e4679563069726b49586447446b5441454c4c733548736474316266746e574e6b7269792d5a6f4548435a4635646865327546386a774258574a3676614a77387367354d2d6532566d6e5f6c716f4d344e7a61457746757a67594b48584e777a315147526830727341545f6d7930))

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