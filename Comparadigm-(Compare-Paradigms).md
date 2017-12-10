Red is a multi-paradigm language. If you want, you can write using a single model, or you can mix and match to make your intent clear, and leverage what each approach has to offer. Solving certain problems lend themselves to a particular paradigm, but there may be tradeoffs. For example, recursion can be wonderful but Red doesn't yet have TCO (Tail Call Optimization), so recursion currently has a cost in Red that another functional language avoids. You also need to consider your target reader. If you're a team of OO or imperative programmers, there is no shame in writing code that you all understand, rather than using a functional approach. 

Red also has ways of doing things that are unique to it, because of its design and core functionality. For example, it has a lot of datatypes, far more than most languages. And it's homoiconic (it is its own data format), so Red data is very easy to process. Add the `parse` function to that, and the ability to create dialects (a.k.a. DSLs) easily, and you have more paradigms.

This page is a place where we can collect examples of how a problem might be solved using different approaches. Bear in mind that the goal is not just to have the shortest solution, but to have the clearest, correct solution that readers can understand. Sometimes we're maintaining code, and sometimes learning from others. Some approaches make it easier to get things right the first time, or to test, or to make fence-post errors stand out (or even eliminate them). Some are more performant, others more robust or flexible. That is to say, we're not looking for a "winner" here. There is no single best solution for every person, team, or context.

When adding new items for comparison, copy the template section and fill it in. If you have a new paradigm you want to include, add it to your own section, but not the general template. We could add other standard paradigms (dataflow, logic, array oriented, etc.), but that will come later, if ever. 

*NOTE: All solutions MUST be implemented in Red. This is not a comparison with other languages, but with what different approaches look like in Red.*

# Template

## Imperative

## Functional

## Object Based or Object Oriented

## Reddish

*(These could be idiomatic solutions, parse-based, dialects, etc., and should have some explanation to go with them, for readers not deeply familiar with Red. That is, say why this might be a good approach, or even why it isn't.)*

----------------
