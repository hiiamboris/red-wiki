The standard JSON codec is meant to be easy to use, and leverages Red for a high level implementation. It is not optimized for high performance. That doesn't mean we can't have other options, but it should work for the vast majority of use cases. What we want to avoid is having a large number of incompatible libraries that differ and specialize in small ways, but without each adding much value outside that.

There are so many JSON libs out there, that we can learn from and perhaps define 3 primary goal-oriented libs:
1) Standard - Trades off performance for readability and ease of implementation. Should just work out of the box, but without many extra options which would complicate it.
2) High performance - Probably done in R/S, perhaps with tricks to reduce memory consumption. Could even leverage things learned from Red's fast lexer, applied to JSON.
3) Flexible - Config options, more parsing control, who knows?

There are many ways to approach these goals, and experiments to show how Red allows new ways to solve this problem will be great. But, ultimately, there should be no more than (arbitrarily) 3 "blessed" libs we guide people to.

Something else to consider is that if we have a standard data model on the Red side, the libs can be intermixed in apps, depending on the needs. Maybe decoding should be fast, but encoding needs to be flexible. 

> January-2020 note: The JSON codec is now a mix of Red, using `parse`, and Red/System (R/S) to optimize string decoding. 

# References, comparisons, benchmarks, etc.

- https://www.json.org/json-en.html
- https://medium.com/tourradar/working-efficiently-with-json-in-go-cb80dcca0466
