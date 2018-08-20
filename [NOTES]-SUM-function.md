# Design Notes On `Sum` and `Average` Functions

Simple `sum` (and `average`) function added to Red. See [PR-3498](https://github.com/red/red/pull/3498)

Currently `sum` doesn't coalesce return value as `float!` in case of integer overflow. This could be done by a `safe` refinement with a cost of performance.

Here you can see a [comparison](https://gist.github.com/endo64/e0ea49bc2fb09d548bad1b077720eda5)

But we generally do not use that kind of error handling, and `add` doesn't have safety too.

Also note that `average` always returns `float!` value.

It could also be done by a HOF accumulator/fold approach. 

Also see @9214's [comment](https://github.com/red/red/pull/3498#issuecomment-413458458)