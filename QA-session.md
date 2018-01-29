# Carl Sassenrath
| Q | A |
|:-:|:-:|
| Any plans to open-source AltMe? | |
| Any way that would make open-sourcing Rebol documentation possible? | |
| When will Carl return to Redbol? | |
| User-defined custom datatypes? | |
| Where can we buy SassenRanch wine? | |
| Does `query` have a modes dialect? | |
| Why `/local` is a refinement? | |
| Who and how came up with ideas of bindology and so-called definitional scoping? | |
| What makes Rebol and Red so small? | |
| Will you release a new 64bit version of R2 with the checksum code corrected to work (see R2 blog on this and R3 implementation)? | |
| Do you have any specific wishes for the Red project? | |
| The WiKipedia mentions that you were interested in parallel processing, why rebol just single-threaded? | |


# Blockchain
| Q | A |
|:-:|:-:|
| Any plans to run a private NEO blockchain? | |
| Have you, in considering the direction you recently announced, looked at Urbit? | |

### ICO
| Q | A |
|:-:|:-:|
| Has a jurisdiction been chosen for the ICO? | We have several options for the jurisdiction, no final decision yet. |
| Let's say the ICO process brings in 10 000 000. How many additional developers do you estimate can Red hire to contribute in an efficient manner? | We plan to split the work in several teams, one would be on `/Core`, one on `/View`, one on `/C3`, another one writing unit tests across all parts. I haven't yet made details plan for that, but about 5-10 extra devs would suffice (more 5 than 10). One of the goal is to free my and Qingtian's time, so we can focus on design and coding the core parts. |
| You will guys need to raise high amount of money, to afford something like even 5 developers for the - let's say - 3 to 5 years period? | Well, we'll scale the spendings according to the funds raised. We need to grow bigger and I think that's the best course of action for us. With the clearer separation between the foundation and the company, it should make the governance and inner workings of the Red codebase management easier to understand and more transparent. |
| Can we get more information on how to get involved in the ICO? | |
| Do you have an estimate how long establishing that foundation will take, couple weeks? | |
| How can we participate in the angel round and pre-sale event? | |
| How will you get the information out to intetested parties? | |
| Why choose ICO instead of traditional funding schemes (Patreon, donations, etc)? | | 
| How are the ICO funds being stored? | |
| Is there an escrow or is the team in control? | |
| Can they provide the addresses so I can see the funds on the block explorer? | |
| Has there been any marketing to the cryptocommunity? | |
| How much money has been raised so far, and where do you expect most of it is coming from? | |

### RCT
| Q | A |
|:-:|:-:|
| What are the transaction fees when RCTs are transferred between people and how are they paid? | |
| Does RCT have a good securities-law plan, or should US residents stay away? | About allowing buyers from US, we are looking into the options for that, but that's not simple as you know already. |
| Isn't there a risk of a party, which has the most buying power, dictating the route of the project? Projects/features, users want to see to be released, can be supported/bought with these coins? | The foundation will set up voting rules that will prevent such things to happen, or at least diminish it. For example, the voting power could be a `log()` function of your RCT amounts. |
| What is the incentive for a potential backer to buy this token? | |

### /Core
| Q | A |
|:-:|:-:|
| How is Red sustained, who currently supports it? | |
| Is stuff like concurrency also required to reach 1.0, and be used on blockchains? And encoding stuff? | Yes, but concurrency does not necessarily imply parallelism, so async tasks should be supported and enough for 1.0. Current I/O already fully support Unicode. For other main encoding formats, we will provide codecs. v0.6.4 already has a Latin1 decoder added recently by @qtxie |
| Is interop with other mainstream libraries/languages (java, python, c#, go,...) planned? | |
| Are there plans for Red to support IPFS? | |
| You said that the current Red roadmap will be delayed by a few weeks. You don't think it will be much more with all activities created by this big move? | |
| Giving recent announcement, it seems that Red drifts further and further from initial idea of Carl. Building blockchain tech without proper GC, or 64 bit support and just basic I/O IMO again putting first things last. And it's worrisome that "full-stack language" and its community may lock itself in one niche. Can you comment on that? | |
| You've mentioned that proposed community restructuration is akin to the one that Clojure project use. Are there any other projects/languages which models you're trying (or want) to follow? | |
| What about webassembly as a compilation target? LLVM? | |

### /C3
| Q | A |
|:-:|:-:|
| Following both planned Red roadmap and doing Red/C3 in one project... isn't it too ambitious? | |
| Some Red users long time express their preferences in Red development, mostly relating to stuff like GC, full IO, simply a core stuff and can see even Android development as some kind of a distraction. On the other hand, Fullstack Technologies has to cover also business side of things. After the Red/C3 announcement, some users fear, there might not be another core technology release in half a year, or more. How to best align community wishes with company commercial needs, especially if the community is still rather small to generate critical mass of quality contributors? | | 