I make a `sequence diagram` from `do/args -r %red.r ...` to compiler a Red script, using [PlantUML](http://plantuml.com/sequence-diagram), in order to see how Red runtime starts.

Here is the PlantUML source, it must exists many mistakes. Wish you could correct my mistakes, thanks!

You can change the source below and copy to [PlantUML online server](http://www.plantuml.com/plantuml/uml) to regenerate a new diagram.

Now I know the Red runtime is stars from [runtime/red.reds](https://github.com/red/red/blob/master/runtime/red.reds#L139), and it's memory model is here [runtime/allocator.reds](https://github.com/red/red/blob/master/runtime/allocator.reds#L84).

Go Red!

## PlantUML source code link

[link](http://www.plantuml.com/plantuml/uml/tLXjSzku4Vui_egDCkxPcb2Kx7CSwowAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86Tzw2A_CaydJnxFK8dwkhULEkfBgG71h9r91MdXh57RUfMULLL5A-AB2PVQ5JlaYAmUGQnYvSqxp8g7AMRzYY37UhuBX5mMRkH05cZyFuP0gAQfZT5ZnLS4-acjl4VfPfJ1ysGcfb4jChQm8ZELm8eU4lW8s12QxmZHFDxgbvmvQTtWs7mwEQFY_xgf7qFnBGnnZGhZYPS3Q5fisIZi1OyRpyDa5hme9XhhQOe2o7y9EY-MtayOQJbR0Nrhx-HMwxTj9ATddAMdsiMBzed7fHhXsmXgtbh0TtTAZGldztPJCSPYI8ZVEjhmgr9A_9CTg1q2lZcXME32hYGkEaWyD7I1dr-eD1JS8US33eHprlepO7UKjxn_Jm518ZKBKJkqZsD2A0eKv8FEbrWnp53h71t7MpdR2QUUfRFCMan8Gq60nx60WNK2riALcIOZNdeGA69N31gMqaneuDupVSFGmS7RQnqBiV_NXgRAy518DFi-2xKl7kREHSdAmnoRxnYtUdnrDWjxYcYbnNFGIKJ4sKO0-zE3i_ioISOaUCosJ-OBTXjYXOdbMhTd7UsMbEn--cbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cvsDdpvWeBBRMfXtfOyQHmU1xGFozr810RPh58eqlcvmW6tCNOUY3QnlIWq4KKCw4P4lC1FW95-TWT4tC10M_30OiA0c-NI5uYnSLenJC3CsYqF3ll_rs_HimXHbCuRkqwMqZy0EI6vLHpzsO3pExPfuuU9wWV_Eunfa7XwakWiyCEoj1YYw2JpJDyV9YYrOdqHLsl-SfViUODpUv9Tw37D4yck-oyKUJiTiX2_HijDVTZ9_I856zpT_0akBjrWAjAGjD5aXIpznQzBypmy9UjDlznDTqr-AoAe5zkZM3lSRsv7pqTkf2rUiMevLoiJGr1JzO-WeyMvo4OURV_nIBE2EQ92Vp7s3lKdm26xopm-oW7DBXO3XCcfFx72XFoUP3PxklipIQ-Ct4r-54R7bPLTn7qEpj7Q3iRdZBVVGzgZR0sN8NET80KuvApyDgQOwuw7M9EyZehKr0z9KzQx2r9gaKUwicjcw5N2TRna-Ecxlr2qie1psZ7pTK9KhB8iN8fQgRfdpROv-sQkf7mkQocm_hYpWkrXkUiMVB_1GQqTHdLJciOtAgzyHd0h1yDM7sOw7o0hCozq8tLpJsIe7NEX_IvdSKmPrD_8uFSl-rkkUf2AlKrFx3gSqFJriXC_N611fsCRnNmfPX1dfPR-n9tjYx2IneBhRDslH2ImYADxffRWh_Fv39biAAtDEzkezbl-mZGRluVsm5jGW8Ea1xnNOkn8MoRYEYlFfFTesy5H5sYtyf4qaOfCRmRTIcDzH_Kx0rwJb8hOBPOW5kzB9UhDRYW9qlAVNxx9gkXNoi5pRtiuZRmJdOUWR775jkNi6iPvvVuElb9rCsiJRmb1csFfXzKO8DClld3QvAIhAefuKcUGzc34DfIUMhUS_nYMEmMruJKEo_j58rWqFqv-9HKtKKzNYyGbR8WsCAnBVeOkhP62vc3mJEiwPtsqtp2vJI1cVA7GP-Ez1jvezGA9e5S0FKXELT0irUkq1LAegEPrI5o51Jjzk4bhgsO-lAXXmCrglR3YtrRHThVxPlzu_TSex1XeMjS5lZqUiTZeyi2wHevv0MKbB-70_FOaqUsZdWHZTQUHKMfTqNRDaP7xXNE_iyYapEn1X_vjA7oxEevu8niP4vWTweMl66ci0VuzylWrNK3-d-EAuj6eLnjGwjrQCBV2tM-Vy0), can be edit.

## sequence diagram([click here to see origin SVG image](http://www.plantuml.com/plantuml/svg/tLXjSzku4Vui_egDCkxPcb2Kx7CSwowAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86Tzw2A_CaydJnxFK8dwkhULEkfBgG71h9r91MdXh57RUfMULLL5A-AB2PVQ5JlaYAmUGQnYvSqxp8g7AMRzYY37UhuBX5mMRkH05cZyFuP0gAQfZT5ZnLS4-acjl4VfPfJ1ysGcfb4jChQm8ZELm8eU4lW8s12QxmZHFDxgbvmvQTtWs7mwEQFY_xgf7qFnBGnnZGhZYPS3Q5fisIZi1OyRpyDa5hme9XhhQOe2o7y9EY-MtayOQJbR0Nrhx-HMwxTj9ATddAMdsiMBzed7fHhXsmXgtbh0TtTAZGldztPJCSPYI8ZVEjhmgr9A_9CTg1q2lZcXME32hYGkEaWyD7I1dr-eD1JS8US33eHprlepO7UKjxn_Jm518ZKBKJkqZsD2A0eKv8FEbrWnp53h71t7MpdR2QUUfRFCMan8Gq60nx60WNK2riALcIOZNdeGA69N31gMqaneuDupVSFGmS7RQnqBiV_NXgRAy518DFi-2xKl7kREHSdAmnoRxnYtUdnrDWjxYcYbnNFGIKJ4sKO0-zE3i_ioISOaUCosJ-OBTXjYXOdbMhTd7UsMbEn--cbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cvsDdpvWeBBRMfXtfOyQHmU1xGFozr810RPh58eqlcvmW6tCNOUY3QnlIWq4KKCw4P4lC1FW95-TWT4tC10M_30OiA0c-NI5uYnSLenJC3CsYqF3ll_rs_HimXHbCuRkqwMqZy0EI6vLHpzsO3pExPfuuU9wWV_Eunfa7XwakWiyCEoj1YYw2JpJDyV9YYrOdqHLsl-SfViUODpUv9Tw37D4yck-oyKUJiTiX2_HijDVTZ9_I856zpT_0akBjrWAjAGjD5aXIpznQzBypmy9UjDlznDTqr-AoAe5zkZM3lSRsv7pqTkf2rUiMevLoiJGr1JzO-WeyMvo4OURV_nIBE2EQ92Vp7s3lKdm26xopm-oW7DBXO3XCcfFx72XFoUP3PxklipIQ-Ct4r-54R7bPLTn7qEpj7Q3iRdZBVVGzgZR0sN8NET80KuvApyDgQOwuw7M9EyZehKr0z9KzQx2r9gaKUwicjcw5N2TRna-Ecxlr2qie1psZ7pTK9KhB8iN8fQgRfdpROv-sQkf7mkQocm_hYpWkrXkUiMVB_1GQqTHdLJciOtAgzyHd0h1yDM7sOw7o0hCozq8tLpJsIe7NEX_IvdSKmPrD_8uFSl-rkkUf2AlKrFx3gSqFJriXC_N611fsCRnNmfPX1dfPR-n9tjYx2IneBhRDslH2ImYADxffRWh_Fv39biAAtDEzkezbl-mZGRluVsm5jGW8Ea1xnNOkn8MoRYEYlFfFTesy5H5sYtyf4qaOfCRmRTIcDzH_Kx0rwJb8hOBPOW5kzB9UhDRYW9qlAVNxx9gkXNoi5pRtiuZRmJdOUWR775jkNi6iPvvVuElb9rCsiJRmb1csFfXzKO8DClld3QvAIhAefuKcUGzc34DfIUMhUS_nYMEmMruJKEo_j58rWqFqv-9HKtKKzNYyGbR8WsCAnBVeOkhP62vc3mJEiwPtsqtp2vJI1cVA7GP-Ez1jvezGA9e5S0FKXELT0irUkq1LAegEPrI5o51Jjzk4bhgsO-lAXXmCrglR3YtrRHThVxPlzu_TSex1XeMjS5lZqUiTZeyi2wHevv0MKbB-70_FOaqUsZdWHZTQUHKMfTqNRDaP7xXNE_iyYapEn1X_vjA7oxEevu8niP4vWTweMl66ci0VuzylWrNK3-d-EAuj6eLnjGwjrQCBV2tM-Vy0))

![sequence diagram](http://www.plantuml.com/plantuml/svg/tLXjSzku4Vui_egDCkxPcb2Kx7CSwowAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86Tzw2A_CaydJnxFK8dwkhULEkfBgG71h9r91MdXh57RUfMULLL5A-AB2PVQ5JlaYAmUGQnYvSqxp8g7AMRzYY37UhuBX5mMRkH05cZyFuP0gAQfZT5ZnLS4-acjl4VfPfJ1ysGcfb4jChQm8ZELm8eU4lW8s12QxmZHFDxgbvmvQTtWs7mwEQFY_xgf7qFnBGnnZGhZYPS3Q5fisIZi1OyRpyDa5hme9XhhQOe2o7y9EY-MtayOQJbR0Nrhx-HMwxTj9ATddAMdsiMBzed7fHhXsmXgtbh0TtTAZGldztPJCSPYI8ZVEjhmgr9A_9CTg1q2lZcXME32hYGkEaWyD7I1dr-eD1JS8US33eHprlepO7UKjxn_Jm518ZKBKJkqZsD2A0eKv8FEbrWnp53h71t7MpdR2QUUfRFCMan8Gq60nx60WNK2riALcIOZNdeGA69N31gMqaneuDupVSFGmS7RQnqBiV_NXgRAy518DFi-2xKl7kREHSdAmnoRxnYtUdnrDWjxYcYbnNFGIKJ4sKO0-zE3i_ioISOaUCosJ-OBTXjYXOdbMhTd7UsMbEn--cbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cvsDdpvWeBBRMfXtfOyQHmU1xGFozr810RPh58eqlcvmW6tCNOUY3QnlIWq4KKCw4P4lC1FW95-TWT4tC10M_30OiA0c-NI5uYnSLenJC3CsYqF3ll_rs_HimXHbCuRkqwMqZy0EI6vLHpzsO3pExPfuuU9wWV_Eunfa7XwakWiyCEoj1YYw2JpJDyV9YYrOdqHLsl-SfViUODpUv9Tw37D4yck-oyKUJiTiX2_HijDVTZ9_I856zpT_0akBjrWAjAGjD5aXIpznQzBypmy9UjDlznDTqr-AoAe5zkZM3lSRsv7pqTkf2rUiMevLoiJGr1JzO-WeyMvo4OURV_nIBE2EQ92Vp7s3lKdm26xopm-oW7DBXO3XCcfFx72XFoUP3PxklipIQ-Ct4r-54R7bPLTn7qEpj7Q3iRdZBVVGzgZR0sN8NET80KuvApyDgQOwuw7M9EyZehKr0z9KzQx2r9gaKUwicjcw5N2TRna-Ecxlr2qie1psZ7pTK9KhB8iN8fQgRfdpROv-sQkf7mkQocm_hYpWkrXkUiMVB_1GQqTHdLJciOtAgzyHd0h1yDM7sOw7o0hCozq8tLpJsIe7NEX_IvdSKmPrD_8uFSl-rkkUf2AlKrFx3gSqFJriXC_N611fsCRnNmfPX1dfPR-n9tjYx2IneBhRDslH2ImYADxffRWh_Fv39biAAtDEzkezbl-mZGRluVsm5jGW8Ea1xnNOkn8MoRYEYlFfFTesy5H5sYtyf4qaOfCRmRTIcDzH_Kx0rwJb8hOBPOW5kzB9UhDRYW9qlAVNxx9gkXNoi5pRtiuZRmJdOUWR775jkNi6iPvvVuElb9rCsiJRmb1csFfXzKO8DClld3QvAIhAefuKcUGzc34DfIUMhUS_nYMEmMruJKEo_j58rWqFqv-9HKtKKzNYyGbR8WsCAnBVeOkhP62vc3mJEiwPtsqtp2vJI1cVA7GP-Ez1jvezGA9e5S0FKXELT0irUkq1LAegEPrI5o51Jjzk4bhgsO-lAXXmCrglR3YtrRHThVxPlzu_TSex1XeMjS5lZqUiTZeyi2wHevv0MKbB-70_FOaqUsZdWHZTQUHKMfTqNRDaP7xXNE_iyYapEn1X_vjA7oxEevu8niP4vWTweMl66ci0VuzylWrNK3-d-EAuj6eLnjGwjrQCBV2tM-Vy0)


## PlantUML source code

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

red.r -> rscompiler : @821> system-dialect/compile/options/loaded src opts result
rscompiler -> rscompiler : @3906> comp-runtime-prolog to logic! loaded all [loaded job-data/3]
rscompiler -> rscompiler : @3906> script: pick [%red.reds %../runtime/red.reds] encap?
rscompiler -> loader : @3906> script:  job loader/process/own script script
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

rscompiler -> rscompiler : compiler/run job loader/process/own script script
rscompiler -> rscompiler : comp-dialect
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