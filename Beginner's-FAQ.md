Have you just started your Red adventure and got a lot of questions? Check some of the frequently asked questions and answers below:

# I need help, what should I do?
Developing a problem-solving mindset is a core skill in becoming a good programmer, so keeping that in mind, try to find the answer you are looking for  following these 7 fail-proof steps:

1. Red's console `help` command is your personal helper! 
Type `help` in the Red console to learn how to access the built-in documentation. Don't be afraid, Red's internal documentation is very concise so you can get a grasp of the main information in a glance. Try typing `help help` to see how `help` itself operates.

2. [Red By Example](https://www.red-by-example.org/) - Here you will find a library of usage examples that hopefully will shed some light on what you are trying to accomplish.

3. [Red Official Documentation](https://github.com/red/docs/blob/master/en/SUMMARY.adoc) -  It's currently under construction, but hopefully what you are looking for is already there.

4. Rebol Docs, specially [Rebol/Core Manual](http://www.rebol.com/docs/core23/rebolcore.html) - Red has a lot in common with its older Rebol brother. While there are many differences between them, specially as Red's development advances, you may find an analogue solution for what you are looking for in the Rebol's realm.   

5. [Red Wiki](https://github.com/red/red/wiki) - It's growing every day and the best part: you can contribute!

6. Search "redlang + your-question" in your favorite web search engine - As the Red community grows, so does Red presence online!

7. [Ask us](https://gitter.im/red/help), we are here to help! 
When everything else fails, the Red community is here for you. Also, make sure to say "hi" in our [Welcome chat room](https://gitter.im/red/red/welcome).

# A command is not working as expected, what should I do?
The issue may have been fixed in one of the [latest automated builds](https://www.red-lang.org/p/download.html), give it a try.

# Why `random` always gives me the same sequence?
That's an intentional design behavior to allow reproducibility by default (some refer to it as a _soft randomness_). In other words, being able to track what sequence random is producing makes it a lot easier when you need to fix some bug in your code.
In order to get a proper random sequence, first you need to initialize the random function with a seed using `random/seed _seed_`. A common reliable seed is the current time, so you could do `random/seed now/time`. After that `random` will generate random sequences calculated using the current time.

# I have been reading Rebol Docs, but not everything works in Red, why?
While both languages share similar look'n'feel, Red is not entirely compatible with Rebol and is not developed with that goal in mind; it tries to take the best parts from both Rebol2 and Rebol3, bringing its own design novelty when needed.

As the Red user manual is not quite finished yet, [REBOL/Core manual](http://www.rebol.com/docs/core23/rebolcore.html) is usually recommended as newcomer's starting point; one can read it with [this](https://github.com/red/red/wiki/%5BDOC%5D-REBOL-Core-Users-Guide-__-A-walkthrough-with-Red) walkthrough at hand and then progress thru a [list](https://github.com/red/red/wiki/%5BLINKS%5D-Learning-resources) of other learning resources if deemed necessary.

For a non-exhaustive list of differences between Rebol and Red, check out [this](https://github.com/red/red/wiki/%5BDOC%5D-Differences-between-Red-and-Rebol) wiki page.

# I'm feeling overwhelmed right now, what should I do?
For anyone new to Red, remember to play, have fun, explore, and give yourself time. Because it looks familiar in many ways, we want to carry over things we know from other programming languages. That works to some extent, then you step into the deep part of the lake and flounder around for a while. Be patient with yourself, enjoy the ride and don't forget that the community is here for you!